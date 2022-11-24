--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.*

--------

# Using the AWS SDK for Rust in AWS Lambda<a name="lambda"></a>

This section demonstrates how to use the SDK in your Lambda code\.

You can use the SDK from within a Lambda function as you would in any other case, with a few added steps\. For further information about setting up a Rust\-based Lambda, see the [aws\-lambda\-rust\-runtime README](https://github.com/awslabs/aws-lambda-rust-runtime#deployment)\.

## Step 1: Create a Lambda<a name="lambda-step1"></a>

The first step is to [create a Lambda from the AWS console](https://docs.aws.amazon.com/lambda/latest/dg/getting-started-create-function.html)\.

For this specific example, we need to [configure the `BUCKET_NAME` environment variable](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html) for the Lambda function, as well as [update the execution role](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html) to [allow creating objects in our S3 bucket](https://aws.amazon.com/premiumsupport/knowledge-center/lambda-execution-role-s3-bucket/)\.

## Step 2: Create a new project and add dependencies<a name="lambda-step2"></a>

This topic shows how you can create a Rust app and upload it\.

### Cargo\.toml<a name="lambda-step2-cargo.toml"></a>

Create a new project with **cargo**, where *NAME* is the name of your Lambda app\.

```
cargo new NAME
```

Add the following dependencies to `Cargo.toml`, where *YOUR\-NAME* is your name, *YOUR\-EMAIL* is your email address, *YOUR\-LICENSE* is the license you use, and *VERSION* is the latest version of the SDK, which you can find on [crates\.io](https://crates.io/search?q=aws-sdk)\.

```
[package]
name = "NAME"
version = "0.1.0"
edition = "2021"
authors = ["YOUR-NAME<YOUR-EMAIL>"]
license = "YOUR-LICENSE"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
tokio = { version = "1", features = ["full"] }
serde = "1"
log = "0.4"
chrono = "0.4"
tracing-subscriber = { version = "0.3", features = ["env-filter"] }
# NOTE: the following crate is not part of the SDK, but it is maintained by AWS.
lambda_runtime = "0.7"
aws-config = "VERSION"
# We are using the Amazon Simple Storage Service (Amazon S3) crate in this example,
# but you can use any SDK crate in your Lambda code.
aws-sdk-s3 = "VERSION"
```

### src/main\.rs<a name="lambda-step2-main.rs"></a>

For this example, we’re going to create an app that receives a request containing some text and then stores that text in Amazon S3 with a name based on the Unix timestamp for when it was received\. It will then reply, reporting whether it succeeded or failed\. When writing a Lambda with Rust, you typically need to define the following\.

1. A struct that represents the data your Lambda will receive\. This struct must implement `serde::Deserialize`\.

1. An async handler function that will perform whatever work you want your Lambda to be responsible for\. This function must have a specific inputs and outputs which we’ll cover in detail later on\.

1. A main function that sets up logging and runs your handler function, routing new requests to it\. This is usually be an async function but it doesn’t have to be\.

1. \(Optional\) A struct that represents data that your lambda returns\. This is typically a `Result` wrapping either a success response or a failure response but can be anything that implements `serde::Serialize`\.

Replace `src/main.rs` with the following code\.

```
use chrono::Utc;
use lambda_runtime::{service_fn, LambdaEvent};
use log::{info, error};
use serde::{Deserialize, Serialize};

#[derive(Deserialize)]
struct Request {
    pub body: String,
}

#[derive(Debug, Serialize)]
struct SuccessResponse {
    pub body: String,
}

#[derive(Debug, Serialize)]
struct FailureResponse {
    pub body: String,
}

// Implement Display for the Failure response so that we can then implement Error.
impl std::fmt::Display for FailureResponse {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        write!(f, "{}", self.body)
    }
}

// Implement Error for the FailureResponse so that we can `?` (try) the Response
// returned by `lambda_runtime::run(func).await` in `fn main`.
impl std::error::Error for FailureResponse {}

type Response = Result<SuccessResponse, FailureResponse>;

#[tokio::main]
async fn main() -> Result<(), lambda_runtime::Error> {
    let func = service_fn(handler);
    lambda_runtime::run(func).await?;

    Ok(())
}

async fn handler(event: LambdaEvent<Request>) -> Response {
    info!("handling a request...");
    let bucket_name = std::env::var("BUCKET_NAME")
        .expect("A BUCKET_NAME must be set in this app's Lambda environment variables.");

    // No extra configuration is needed as long as your Lambda has
    // the necessary permissions attached to its role.
    let config = aws_config::load_from_env().await;
    let s3_client = aws_sdk_s3::Client::new(&config);
    // Generate a filename based on when the request was received.
    let filename = format!("{}.txt", Utc::now().timestamp());

    let _ = s3_client
        .put_object()
        .bucket(bucket_name)
        .body(event.payload.body.as_bytes().to_owned().into())
        .key(&filename)
        .content_type("text/plain")
        .send()
        .await
        .map_err(|err| {
            // In case of failure, log a detailed error to CloudWatch.
            error!(
                "failed to upload file '{}' to S3 with error: {}",
                &filename, err
            );
            // The sender of the request receives this message in response.
            FailureResponse {
                body: format!("The lambda encountered an error and your message was not saved: {}", err),
            }
        })?;

    info!(
        "Successfully stored the incoming request in S3 with the name '{}'",
        &filename
    );

    Ok(SuccessResponse {
        body: format!(
            "the lambda has successfully stored the your request in S3 with name '{}'",
            filename
        ),
    })
}
```

## Step 3: Package and upload the app<a name="lambda-step3"></a>

This topic shows you how to package your app and upload it\.

To be usable as a Lambda, your Rust app must be compiled for the **x86\_64\-unknown\-linux\-musl** target\. This typically means cross compiling the app\. You might encounter build errors if your app needs a certain dependency, but that dependency isn’t available on your system\.

This section shows two approaches to cross compiling that you can try\.

### Rustup approach<a name="lambda-step3-x1"></a>

This approach uses the [Rustup](https://rustup.rs) toolchain to cross\-compile your app\.

**Note**
If you’re developing on a Mac, you must install the MUSL build tooling provided by [musl\-cross](https://github.com/FiloSottile/homebrew-musl-cross) and expose it as described in [this issue](https://github.com/awslabs/aws-sdk-rust/issues/186#issuecomment-898442351)\. On Linux, you will need to install `musl`, `musl-dev`, or `musl-tools` depending on your distribution\.

1. Install the **x86\_64\-unknown\-linux\-musl** toolchain with **Rustup** by running:

   ```
   rustup target add x86_64-unknown-linux-musl
   ```

1. Build the app by running:

   ```
   cargo build --release --target x86_64-unknown-linux-musl
   ```

If this approach fails, try the next one\.

### Container approach<a name="lambda-step3-x2"></a>

This approach requires that you install either [Docker](https://www.docker.com) or [Podman](https://podman.io)\.

1. Install rust\-embedded/cross as a cargo command by running:

   ```
   cargo install cross
   ```

1. Build the app by running:

   ```
   cross build --release --target x86_64-unknown-linux-musl
   ```

If your build is still failing at this point, it’s possible one of your dependencies isn’t compatible with MUSL Linux or requires some special setup\.

### Packaging and uploading your app<a name="lambda-packaging"></a>

1. Rename the app binary you just built to `bootstrap`\. On Linux, OS X, or Unix you can accomplish this by running:

   ```
   cp target/x86_64-unknown-linux-musl/release/your_lambda_app_name bootstrap
   ```

   On Microsoft Windows run:

   ```
   cp target\x86_64-unknown-linux-musl\release\your_lambda_app_name bootstrap
   ```

1. Place your renamed binary in a zip file\. On Linux, OS X, or Unix you can accomplish this by running:

   ```
   zip lambda.zip bootstrap
   ```

   On Microsoft Windows, right\-click **bootstrap** in Windows Explorer, select **Send to**, and then select **Compressed \(zipped\) folder**\. Rename **bootstrap\.zip** file as **lambda\.zip**\.

1. Upload **lambda\.zip** to your lambda using any of the following\.
   + The [web console](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-package.html)
   + The [AWS Command Line Interface \(AWS CLI\)](https://github.com/awslabs/aws-lambda-rust-runtime#aws-cli)
   + The [AWS Cloud Development Kit \(AWS CDK\)](https://aws.amazon.com/cdk)

You’ve created and deployed a new Rust\-based Lambda that’s ready to begin accepting requests\. You can use [AWS Lambda’s](https://aws.amazon.com/lambda/) [Test feature](https://docs.aws.amazon.com/lambda/latest/dg/testing-functions.html) to try it out\. The following test payload can be used for our function:

   ```
   {"body":"Hello, Rust in Lambda!"}
   ```

This section has covered the process of building and packaging your app at a high level\. If you still have some unanswered questions, take a look at the detailed documentation in the [**aws\-lambda\-rust\-runtime** repository](https://github.com/awslabs/aws-lambda-rust-runtime)\.