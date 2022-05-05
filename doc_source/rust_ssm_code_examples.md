--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Systems Manager examples using SDK for Rust<a name="rust_ssm_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Systems Manager\.

*Actions* are code excerpts that show you how to call individual Systems Manager functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Systems Manager functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c69c13)

## Actions<a name="w14aac14b9c69c13"></a>

### Add a parameter<a name="ssm_PutParameter_rust_topic"></a>

The following code example shows how to add a Systems Manager parameter\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn make_parameter(
    client: &Client,
    name: &str,
    value: &str,
    description: &str,
) -> Result<(), Error> {
    let resp = client
        .put_parameter()
        .overwrite(true)
        .r#type(ParameterType::String)
        .name(name)
        .value(value)
        .description(description)
        .send()
        .await?;

    println!("Success! Parameter now has version: {}", resp.version());

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ssm#code-examples)\. 
+  For API details, see [PutParameter](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Get parameters information<a name="ssm_DescribeParameters_rust_topic"></a>

The following code example shows how to get Systems Manager parameters information\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_parameters(client: &Client) -> Result<(), Error> {
    let resp = client.describe_parameters().send().await?;

    for param in resp.parameters().unwrap().iter() {
        println!("  {}", param.name().unwrap_or_default());
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ssm#code-examples)\. 
+  For API details, see [DescribeParameters](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 