--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# AWS STS examples using SDK for Rust<a name="rust_sts_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with AWS STS\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)

## Actions<a name="actions"></a>

### Assume a role<a name="sts_AssumeRole_rust_topic"></a>

The following code example shows how to assume a role with AWS STS\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sts/#code-examples)\. 
  

```
async fn assume_role(
    config: &SdkConfig,
    region: Region,
    role_name: String,
    session_name: Option<String>,
) -> Result<(), Error> {
    match config.credentials_provider() {
        Some(credential) => {
            let provider = aws_config::sts::AssumeRoleProvider::builder(role_name)
                .region(region)
                .session_name(session_name.unwrap_or_else(|| String::from("rust-assume-role")))
                .build(credential.clone());
            let local_config = aws_config::from_env()
                .credentials_provider(provider)
                .load()
                .await;
            let client = Client::new(&local_config);
            let req = client.get_caller_identity();
            let resp = req.send().await;
            match resp {
                Ok(e) => {
                    println!("UserID :               {}", e.user_id().unwrap_or_default());
                    println!("Account:               {}", e.account().unwrap_or_default());
                    println!("Arn    :               {}", e.arn().unwrap_or_default());
                }
                Err(e) => println!("{:?}", e),
            }
        }
        None => {
            println!("No config provided");
        }
    }
    Ok(())
}
```
+  For API details, see [AssumeRole](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 