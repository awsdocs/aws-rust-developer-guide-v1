--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# DynamoDB local with the AWS SDK for Rust<a name="dynamodb-local"></a>

This section describes how to use [Amazon DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html), which is a local version of the DynamoDB service\. You can use DynamoDB Local by providing a static endpoint pointing to `http://localhost:8080`\. You must provide an AWS region and credentials, however, they need not be valid\. One way to do this is by providing a **localstack** profile in your `config` file \(`~/.aws/config` on Linux, OS X, and Linux; %userprofile%\\\.aws\\config on Microsoft Windows\), as shown, and setting **AWS\_PROFILE=localstack** when running your application\.

```toml
[profile localstack]
region = us-east-1
aws_access_key_id = AKIDLOCALSTACK
aws_secret_access_key = localstacksecret
```

The following code example demonstrates how to use DynamoDB Local to retrieve a list of your local tables\.

```rust
use aws_sdk_dynamodb::{Client, Endpoint, Error};
use http::Uri;

/// Lists your tables in DynamDB local.
#[tokio::main]
async fn main() -> Result<(), Error> {
    // Select a profile by setting the `AWS_PROFILE` environment variable.
    let config = aws_config::load_from_env().await;
    let dynamodb_local_config = aws_sdk_dynamodb::config::Builder::from(&config)
        .endpoint_resolver(
            // 8000 is the default dynamodb port
            Endpoint::immutable(Uri::from_static("http://localhost:8000")),
        )
        .build();

    let client = Client::from_conf(dynamodb_local_config);

    let resp = client.list_tables().send().await?;

    println!("Tables:");

    let names = resp.table_names().unwrap_or_default();

    for name in names {
        println!("  {}", name);
    }

    println!();
    println!("Found {} tables", names.len());

    Ok(())
}
```