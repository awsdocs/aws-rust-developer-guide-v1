--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Secrets Manager examples using SDK for Rust<a name="rust_secrets-manager_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Secrets Manager\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)

## Actions<a name="actions"></a>

### Create a secret<a name="secrets-manager_CreateSecret_rust_topic"></a>

The following code example shows how to create a Secrets Manager secret\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/secretsmanager#code-examples)\. 
  

```
async fn make_secret(client: &Client, name: &str, value: &str) -> Result<(), Error> {
    client
        .create_secret()
        .name(name)
        .secret_string(value)
        .send()
        .await?;

    println!("Created secret");

    Ok(())
}
```
+  For API details, see [CreateSecret](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Get a secret value<a name="secrets-manager_GetSecretValue_rust_topic"></a>

The following code example shows how to get a Secrets Manager secret value\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/secretsmanager#code-examples)\. 
  

```
async fn show_secret(client: &Client, name: &str) -> Result<(), Error> {
    let resp = client.get_secret_value().secret_id(name).send().await?;

    println!("Value: {}", resp.secret_string().unwrap_or("No value!"));

    Ok(())
}
```
+  For API details, see [GetSecretValue](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### List secrets<a name="secrets-manager_ListSecrets_rust_topic"></a>

The following code example shows how to list Secrets Manager secrets\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/secretsmanager#code-examples)\. 
  

```
async fn show_secrets(client: &Client) -> Result<(), Error> {
    let resp = client.list_secrets().send().await?;

    println!("Secret names:");

    let secrets = resp.secret_list().unwrap_or_default();
    for secret in secrets {
        println!("  {}", secret.name().unwrap_or("No name!"));
    }

    println!("Found {} secrets", secrets.len());

    Ok(())
}
```
+  For API details, see [ListSecrets](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 