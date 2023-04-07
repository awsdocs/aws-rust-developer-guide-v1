--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Localstack with the AWS SDK for Rust<a name="localstack"></a>

This section describes how you can use [LocalStack](https://github.com/localstack/localstack) with the SDK\.

LocalStack is a cloud service emulator that runs in a single container on your computer\. You can use the Rust SDK with LocalStack by setting a custom endpoint, as shown in the following code example\. The example configures clients for SQS and S3 to use the LocalStack endpoint if the `LOCALSTACK` environment variable is `true`\.

## Cargo\.toml<a name="localstack-cargo"></a>

Specify the SDK crates in `Cargo.toml`\. Note that this specifies to use the developer preview version of the crates\. Check [crates\.io](https://crates.io/) for the latest version\.

```toml
[package]
name = "localstack-example"
version = "0.1.0"
authors = ["Doug Schwartz <dougsch@amazon.com>"]
edition = "2018"

[dependencies]
aws-config = { git = "https://github.com/awslabs/aws-sdk-rust", branch = "next" }
aws-sdk-s3 = { git = "https://github.com/awslabs/aws-sdk-rust", branch = "next" }
aws-sdk-sqs = { git = "https://github.com/awslabs/aws-sdk-rust", branch = "next" }
aws-types = { git = "https://github.com/awslabs/aws-sdk-rust", branch = "next" }
aws-smithy-http = { git = "https://github.com/awslabs/aws-sdk-rust", branch = "next" }
tokio = { version = "1", features = ["full"] }
http = "0.2"
tracing-subscriber = { version = "0.3.5", features = ["env-filter"] }
```

## src/main\.rs<a name="localstack-main"></a>

```rust
use aws_smithy_http::endpoint::Endpoint;
use http::Uri;
use std::error::Error;

#[tokio::main]
async fn main() -> Result<(), Box<dyn Error>> {
    tracing_subscriber::fmt::init();

    let shared_config = aws_config::from_env().load().await;
    let sqs_client = sqs_client(&shared_config);
    let s3_client = s3_client(&shared_config);

    let resp = s3_client.list_buckets().send().await?;
    let buckets = resp.buckets().unwrap_or_default();
    let num_buckets = buckets.len();

    println!("Buckets:");
    for bucket in buckets {
        println!("  {}", bucket.name().as_deref().unwrap_or_default());
    }

    println!();
    println!("Found {} buckets.", num_buckets);
    println!();

    let repl = sqs_client.list_queues().send().await?;
    let queues = repl.queue_urls().unwrap_or_default();
    let num_queues = queues.len();

    println!("Queue URLs:");
    for queue in queues {
        println!("  {}", queue);
    }

    println!();
    println!("Found {} queues.", num_queues);
    println!();

    if use_localstack() {
        println!("Using the local stack.");
    }

    Ok(())
}

/// If LOCALSTACK environment variable is true, use LocalStack endpoints.
/// You can use your own method for determining whether to use LocalStack endpoints.
fn use_localstack() -> bool {
    std::env::var("LOCALSTACK").unwrap_or_default() == "true"
}

fn localstack_endpoint() -> Endpoint {
    Endpoint::immutable(Uri::from_static("http://localhost:4566/"))
}

fn sqs_client(conf: &aws_types::SdkConfig) -> aws_sdk_sqs::Client {
    let mut sqs_config_builder = aws_sdk_sqs::config::Builder::from(conf);
    if use_localstack() {
        sqs_config_builder = sqs_config_builder.endpoint_resolver(localstack_endpoint())
    }
    aws_sdk_sqs::Client::from_conf(sqs_config_builder.build())
}

fn s3_client(conf: &aws_types::SdkConfig) -> aws_sdk_s3::Client {
    let mut s3_config_builder = aws_sdk_s3::config::Builder::from(conf);
    if use_localstack() {
        s3_config_builder = s3_config_builder.endpoint_resolver(localstack_endpoint());
    }
    aws_sdk_s3::Client::from_conf(s3_config_builder.build())
}
```