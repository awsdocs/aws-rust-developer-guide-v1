--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Logging in the AWS SDK for Rust<a name="logging"></a>

The Rust SDK uses the [tracing](http://tracing.rs/) framework for logging\.

## Basic logging<a name="logger"></a>

Since tracing works with the facade exposed by the [log](https://docs.rs/log/0.4.14/log) crate, you can use crates like [env\_logger](https://crates.io/crates/env_logger) to easily see log messages emitted by the SDK\.

The first step is to add a reference to **env\_logger** in your **Cargo\.toml** file:

```
env_logger = "0.9.0"
```

### Full code listing<a name="logger-cargo-toml"></a>

Here is the full listing of **Cargo\.toml**, where VERSION is the latest version of [aws\-config](https://crates.io/crates/aws-config) and [aws\-sdk\-dynamodb](https://crates.io/crates/aws-sdk-dynamodb) on **crates\.io**\.

```
[package]
authors = ["Russell Cohen <rcoh@amazon.com>", "Doug Schwartz <dougsch@amazon.com>"]
edition = "2018"
name = "logging-example"
version = "0.1.0"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
aws-config = "VERSION"
aws-sdk-dynamodb = "VERSION"
env_logger = "0.9.0"
structopt = { version = "0.3", default-features = false }
tokio = { version = "1", features = ["full"] }
```

Then, in your Rust code, initialize the logger in the `main` function before you call any SDK operation:

```
env_logger::init();
```

### Full code listing<a name="logger-code"></a>

```
use aws_config::meta::region::RegionProviderChain;
use aws_sdk_dynamodb::{Client, Error, Region, PKG_VERSION};
use structopt::StructOpt;

#[derive(Debug, StructOpt)]
struct Opt {
    /// The AWS Region.
    #[structopt(short, long)]
    region: Option<String>,

    /// Whether to display additional information.
    #[structopt(short, long)]
    verbose: bool,
}

#[tokio::main]
async fn main() -> Result<(), Error> {
    env_logger::init();

    let Opt { region, verbose } = Opt::from_args();

    let region_provider = RegionProviderChain::first_try(region.map(Region::new))
        .or_default_provider()
        .or_else(Region::new("us-west-2"));

    println!();

    if verbose {
        println!("DynamoDB client version: {}", PKG_VERSION);
        println!(
            "Region:                  {}",
            region_provider.region().await.unwrap().as_ref()
        );

        println!();
    }

    let shared_config = aws_config::from_env().region(region_provider).load().await;
    let client = Client::new(&shared_config);

    let resp = client.list_tables().send().await?;

    println!("Tables:");

    let names = resp.table_names.unwrap_or_default();
    let len = names.len();

    for name in names {
        println!("  {}", name);
    }

    println!("Found {} tables", len);
    Ok(())
}
```

When you run this code, you won't see any logging information\. To enable the display of logging information, use the following command, which sets the log level to **debug**:

```
RUST_LOG=aws_config=debug cargo run
```

You can redirect the logs to the file **log\.txt** by using the following command line:

```
RUST_LOG=aws_config=debug cargo run 2> log.txt
```

## Structured Logging: Tracking Spans and Events<a name="tracing"></a>

Using **env\_logger** is fine for basic logging, however, the SDK also tracks spans and events\. To see this more detailed information, we recommend using tracing\-specific libraries, such as **tracing\_subscriber** or **tracing\_appender**\.

Add the tracing library to your **Cargo\.toml** file:

```
tracing-subscriber = { version = "0.3", features = ["env-filter"] }
```

Then, in your Rust code, initialize the logger in the `main` function before you call any SDK operation:

```
tracing_subscriber::fmt::init();
```

### Full code listing<a name="tracing-code"></a>

```
use aws_config::meta::region::RegionProviderChain;
use aws_sdk_dynamodb::{Client, Error, Region, PKG_VERSION};
use structopt::StructOpt;

#[derive(Debug, StructOpt)]
struct Opt {
    /// The AWS Region.
    #[structopt(short, long)]
    region: Option<String>,

    /// Whether to display additional information.
    #[structopt(short, long)]
    verbose: bool,
}

#[tokio::main]
async fn main() -> Result<(), Error> {
    tracing_subscriber::fmt::init();

    let Opt { region, verbose } = Opt::from_args();

    let region_provider = RegionProviderChain::first_try(region.map(Region::new))
        .or_default_provider()
        .or_else(Region::new("us-west-2"));

    println!();

    if verbose {
        println!("DynamoDB client version: {}", PKG_VERSION);
        println!(
            "Region:                  {}",
            region_provider.region().await.unwrap().as_ref()
        );

        println!();
    }

    let shared_config = aws_config::from_env().region(region_provider).load().await;
    let client = Client::new(&shared_config);

    let resp = client.list_tables().send().await?;

    println!("Tables:");

    let names = resp.table_names.unwrap_or_default();
    let len = names.len();

    for name in names {
        println!("  {}", name);
    }

    println!("Found {} tables", len);
    Ok(())
}
```

**Note**  
If you only see info logs, make sure you have the **env\-filter** feature enabled in your **Cargo\.toml** file:  

```
tracing-subscriber = { version = "0.3", features = ["env-filter"] }
```

## Filtering log messages<a name="logging-filtering"></a>

If you use a crate that supports an environment filtering, such as **env\_logger** or **tracing\_subscriber**, you can filter logs by module\. The following table describes some of the modules that you can use to filter log messages:


| Prefix | Description | 
| --- | --- | 
|  aws\_smithy\_http=trace  |  Request and response wire logging  | 
|  aws\_config=debug  |  Credentials loading  | 
|  aws\_sigv4=trace  |  Request signing and canonical requests  | 