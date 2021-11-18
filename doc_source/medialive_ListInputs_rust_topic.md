--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List inputs<a name="medialive_ListInputs_rust_topic"></a>

The following code example shows how to list your MediaLive inputs\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
List your MediaLive input names and ARNs in the Region\.  

```
async fn show_inputs(client: &Client) -> Result<(), Error> {
    let input_list = client.list_inputs().send().await?;

    for i in input_list.inputs.unwrap_or_default() {
        let input_arn = i.arn.as_deref().unwrap_or_default();
        let input_name = i.name.as_deref().unwrap_or_default();

        println!("Input Name : {}", input_name);
        println!("Input ARN : {}", input_arn);
        println!();
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/medialive#code-examples)\. 
+  For API details, see [ListInputs](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 