--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Amazon Cognito Identity Provider examples using SDK for Rust<a name="rust_cognito-identity-provider_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Amazon Cognito Identity Provider\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)

## Actions<a name="actions"></a>

### List the user pools<a name="cognito-identity-provider_ListUserPools_rust_topic"></a>

The following code example shows how to list the Amazon Cognito user pools\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/cognitoidentityprovider#code-examples)\. 
  

```
async fn show_pools(client: &Client) -> Result<(), Error> {
    let response = client.list_user_pools().max_results(10).send().await?;
    if let Some(pools) = response.user_pools() {
        println!("User pools:");
        for pool in pools {
            println!("  ID:              {}", pool.id().unwrap_or_default());
            println!("  Name:            {}", pool.name().unwrap_or_default());
            println!("  Status:          {:?}", pool.status());
            println!("  Lambda Config:   {:?}", pool.lambda_config().unwrap());
            println!(
                "  Last modified:   {}",
                pool.last_modified_date().unwrap().to_chrono_utc()?
            );
            println!(
                "  Creation date:   {:?}",
                pool.creation_date().unwrap().to_chrono_utc()
            );
            println!();
        }
    }
    println!("Next token: {}", response.next_token().unwrap_or_default());

    Ok(())
}
```
+  For API details, see [ListUserPools](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 