--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Using the AWS SDK for Rust in AWS Lambda<a name="lambda"></a>

This section demonstrates how to use the SDK in your Lambda code\.

You can use the SDK from within a Lambda function as you would in any other case, with a few added steps\. For further information about setting up a Rust\-based Lambda, see the [aws\-lambda\-rust\-runtime README](https://github.com/awslabs/aws-lambda-rust-runtime#2-deploying-the-binary-to-aws-lambda)\. 

## Step 1: Create a new project and add dependencies<a name="lambda-step1"></a>

Create a new project with **cargo**:

```
cargo new s3-example
```

Add the following dependencies to `Cargo.toml`, where *YOUR\-NAME* is your name, *YOUR\-EMAIL* is your email address, *YOUR\-LICENSE* is the license you use.

Keep in mind that the dependency versions might need to be updated since the publication of this tutorial. You can find the latest version of the SDK on [crates\.io](https://crates.io/search?q=aws-sdk)\.

```
[package]
name = "s3-example"
version = "0.1.0"
edition = "2021"
authors = ["YOUR-NAME<YOUR-EMAIL>"]
license = "YOUR-LICENSE"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
aws-config = "0.49.0"
aws-sdk-s3 = "0.19.0"

lambda_runtime = "0.6.1"
serde = "1.0.136"
serde_json = "1.0.85"
tokio = { version = "1", features = ["macros"] }
tracing = { version = "0.1", features = ["log"] }
tracing-subscriber = { version = "0.3", default-features = false, features = ["fmt"] }
```

### src/main\.rs<a name="lambda-step2-main.rs"></a>

We’re going to create an app that receives a request containing some text and then stores that text in Amazon S3 with a name based on the Unix timestamp for when it was received\. It will then reply, reporting whether it succeeded or failed\. When writing a Lambda with Rust, you typically need to define the following\.

1. A struct that represents the data your Lambda will receive\. This struct must implement `serde::Deserialize`\.

1. An async handler function that will perform whatever work you want your Lambda to be responsible for\. This function must have a specific inputs and outputs which we’ll cover in detail later on\.

1. A main function that sets up logging and runs your handler function, routing new requests to it\.

1. \(Optional\) A struct that represents data that your lambda returns\. This is typically a `Result` wrapping either a success response or a failure response. The success response can be anything that implements `serde::Serialize`\, while the failure response can be anything that implements `std::fmt::Debug + std::fmt::Display`.

Replace `src/main.rs` with the following code\.

```
use lambda_runtime::{service_fn, Error, LambdaEvent};
use serde::{Deserialize, Serialize};
use std::time::SystemTime;

#[derive(Deserialize)]
struct Request {
    body: String,
}

#[derive(Debug, Serialize)]
struct Response {
    req_id: String,
    body: String,
}

impl std::fmt::Display for Response {
    /// Display the response struct as a JSON string
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        let err_as_json = serde_json::json!(self).to_string();
        write!(f, "{err_as_json}")
    }
}

impl std::error::Error for Response {}

#[tracing::instrument(skip(s3_client, event), fields(req_id = %event.context.request_id))]
async fn put_object(
    s3_client: &aws_sdk_s3::Client,
    bucket_name: &str,
    event: LambdaEvent<Request>,
) -> Result<Response, Error> {
    tracing::info!("handling a request");

    // Generate a filename based on when the request was received.
    let timestamp = SystemTime::now()
        .duration_since(SystemTime::UNIX_EPOCH)
        .map(|n| n.as_secs())
        .expect("SystemTime before UNIX EPOCH, clock might have gone backwards");
    let filename = format!("{timestamp}.txt");

    let response = s3_client
        .put_object()
        .bucket(bucket_name)
        .body(event.payload.body.as_bytes().to_owned().into())
        .key(&filename)
        .content_type("text/plain")
        .send()
        .await;

    match response {
        Ok(_) => {
            tracing::info!(
                filename = %filename,
                "data successfully stored in S3",
            );

            // Return `Response` (it will be serialized to JSON automatically by the runtime)
            Ok(Response {
                req_id: event.context.request_id,
                body: format!(
                    "the lambda has successfully stored the your data in S3 with name '{filename}'"
                ),
            })
        }
        Err(err) => {
            // In case of failure, log a detailed error to CloudWatch.
            tracing::error!(
                err = %err,
                filename = %filename,
                "failed to upload data to S3"
            );

            Err(Box::new(Response {
                req_id: event.context.request_id,
                body: "The lambda encountered an error and your data was not saved".to_owned(),
            }))
        }
    }
}

#[tokio::main]
async fn main() -> Result<(), Error> {
    tracing_subscriber::fmt()
        .with_max_level(tracing::Level::INFO)
        // disable printing the name of the module in every log line.
        .with_target(false)
        // disabling time is handy because CloudWatch will add the ingestion time.
        .without_time()
        .init();

    let bucket_name = std::env::var("BUCKET_NAME")
        .expect("A BUCKET_NAME must be set in this app's Lambda environment variables.");

    // Initialize the client here to be able to reuse it across
    // different invocations.
    //
    // No extra configuration is needed as long as your Lambda has
    // the necessary permissions attached to its role.
    let config = aws_config::load_from_env().await;
    let s3_client = aws_sdk_s3::Client::new(&config);

    lambda_runtime::run(service_fn(|event: LambdaEvent<Request>| async {
        put_object(&s3_client, &bucket_name, event).await
    }))
    .await
}
```

## Step 3: Package and upload the app<a name="lambda-step3"></a>

This topic shows you how to package your app and upload it\.

To be usable as a Lambda, your Rust app must be compiled for a Linux target, either X86-64, or ARM64\. This typically means cross compiling the app\. You might encounter build errors if your app needs a certain dependency, but that dependency isn’t available on your system\.

This section uses [Cargo Lambda](https:://www.cargo-lambda.info) to build your function. Cargo Lambda is a project maintained by the community that works across Windows, MacOS, and Linux. Refer to [Cargo Lambda's installation instructions](https://www.cargo-lambda.info/guide/installation.html) to learn how to install this tool before continuing with the tutorial.

The advantage of using this project is that you can cross-compile to either Linux target with one single tool.

1. Build the app by running **cargo lambda build** in the project's root directory:

```
cargo lambda build --release --output-format zip
```

1. If you want to compile your app to use AWS Graviton, just add the `--arm64` flag to the previous command:

```
cargo lambda build --release --output-format zip --arm64
```

These previous commands will create a ZIP file with your application's binary in your app's target directory.

1. Upload **target/lambda/s3-example/bootstrap\.zip** to AWS Lambda using any of the following\.
   + The [web console](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-package.html)
   + The [AWS Command Line Interface \(AWS CLI\)](https://github.com/awslabs/aws-lambda-rust-runtime#aws-cli)
   + The [AWS Cloud Development Kit \(AWS CDK\)](https://aws.amazon.com/cdk)

You’ve created and deployed a new Rust\-based Lambda that’s ready to begin accepting requests\. You can use [Amazon API Gateway's](https://aws.amazon.com/api-gateway) [Test feature](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-test-method.html) to try it out\.

This section has covered the process of building and packaging your app at a high level\. If you still have some unanswered questions, take a look at the detailed documentation in the **aws\-lambda\-rust\-runtime** repo under the [Deployment section](https://github.com/awslabs/aws-lambda-rust-runtime#2-deploying-the-binary-to-aws-lambda)\.