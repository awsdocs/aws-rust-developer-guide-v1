--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Get parameters information<a name="ssm_DescribeParameters_rust_topic"></a>

The following code example shows how to get Systems Manager parameters information\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_parameters(client: &Client) -> Result<(), Error> {
    let resp = client.describe_parameters().send().await?;

    for param in resp.parameters.unwrap().iter() {
        println!("  {}", param.name.as_deref().unwrap_or_default());
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/ssm#code-examples)\. 
+  For API details, see [DescribeParameters](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 