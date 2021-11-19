--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Localstack with the AWS SDK for Rust<a name="localstack"></a>

This section describes how you can use [LocalStack](https://github.com/localstack/localstack) with the SDK\.

LocalStack is a cloud service emulator that runs in a single container on your computer\. You can use the Rust SDK with LocalStack by setting a custom endpoint, as shown in the following code example\. The example configures clients for SQS and S3 to use the LocalStack endpoint if the `LOCALSTACK` environment variable is `true`\.

## Cargo\.toml<a name="localstack-cargo"></a>

Specify the SDK crates in `Cargo.toml`, where *VERSION* is the latest version of the crate on [crates\.io](https://crates.io/)\.

```
[package]
name = "localstack-example"
version = "0.1.0"
edition = "2018"

[dependencies]
aws-sdk-sqs = "VERSION"
aws-sdk-s3 = "VERSION"
aws-config = "VERSION"
aws-types = "VERSION"
aws-smithy-http = "VERSION"
tokio = { version = "1", features = ["full"] }
tracing-subscriber = "0.2"
http = "0.2"
```

## src/main\.rs<a name="localstack-main"></a>

```
use aws_sdk_sqs::Region;
use aws_smithy_http::endpoint::Endpoint;
use http::Uri;
use std::error::Error;

#[tokio::main]
async fn main() -> Result<(), Box<dyn Error>> {
    tracing_subscriber::fmt::init();
    let shared_config = aws_config::from_env().load().await;
    let sqs_client = sqs_client(&shared_config);
    let s3_client = s3_client(&shared_config);

    let buckets = s3_client.list_buckets().send().await?;
    println!("s3 buckets: {:?}", buckets.buckets().unwrap_or_default());

    let queues = sqs_client.list_queues().send().await?;
    println!("SQS queues: {:?}", queues.queue_urls().unwrap_or_default());
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

fn sqs_client(conf: &aws_types::config::Config) -> aws_sdk_sqs::Client {
    let mut sqs_config_builder = aws_sdk_sqs::config::Builder::from(conf);
    if use_localstack() {
        sqs_config_builder = sqs_config_builder.endpoint_resolver(localstack_endpoint())
    }
    aws_sdk_sqs::Client::from_conf(sqs_config_builder.build())
}

fn s3_client(conf: &aws_types::config::Config) -> aws_sdk_s3::Client {
    let mut s3_config_builder = aws_sdk_s3::config::Builder::from(conf);
    if use_localstack() {
        s3_config_builder = s3_config_builder.endpoint_resolver(localstack_endpoint());
    }
    aws_sdk_s3::Client::from_conf(s3_config_builder.build())
}
```