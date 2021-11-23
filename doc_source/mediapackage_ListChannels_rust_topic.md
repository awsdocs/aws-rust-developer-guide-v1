--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List channels<a name="mediapackage_ListChannels_rust_topic"></a>

The following code example shows how to list your MediaPackage channels\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
List channel ARNs and descriptions\.  

```
async fn show_channels(client: &Client) -> Result<(), Error> {
    let list_channels = client.list_channels().send().await?;

    println!("Channels:");

    for c in list_channels.channels.unwrap_or_default() {
        let description = c.description.as_deref().unwrap_or_default();
        let arn = c.arn.as_deref().unwrap_or_default();

        println!("  Description : {}", description);
        println!("  ARN :         {}", arn);
        println!();
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/mediapackage#code-examples)\. 
+  For API details, see [ListChannels](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 