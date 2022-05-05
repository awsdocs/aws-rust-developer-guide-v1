--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# MediaPackage examples using SDK for Rust<a name="rust_mediapackage_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with MediaPackage\.

*Actions* are code excerpts that show you how to call individual MediaPackage functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple MediaPackage functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c47c13)

## Actions<a name="w14aac14b9c47c13"></a>

### List channels<a name="mediapackage_ListChannels_rust_topic"></a>

The following code example shows how to list your MediaPackage channels\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
List channel ARNs and descriptions\.  

```
async fn show_channels(client: &Client) -> Result<(), Error> {
    let list_channels = client.list_channels().send().await?;

    println!("Channels:");

    for c in list_channels.channels().unwrap_or_default() {
        let description = c.description().unwrap_or_default();
        let arn = c.arn().unwrap_or_default();

        println!("  Description : {}", description);
        println!("  ARN :         {}", arn);
        println!();
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/mediapackage#code-examples)\. 
+  For API details, see [ListChannels](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### List your origin endpoints<a name="mediapackage_ListOriginEndpoints_rust_topic"></a>

The following code example shows how to list your MediaPackage origin endpoints\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
List your endpoint descriptions and URLs\.  

```
async fn show_endpoints(client: &Client) -> Result<(), Error> {
    let or_endpoints = client.list_origin_endpoints().send().await?;

    println!("Endpoints:");

    for e in or_endpoints.origin_endpoints().unwrap_or_default() {
        let endpoint_url = e.url().unwrap_or_default();
        let endpoint_description = e.description().unwrap_or_default();
        println!("  Description: {}", endpoint_description);
        println!("  URL :        {}", endpoint_url);
        println!();
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/mediapackage#code-examples)\. 
+  For API details, see [ListOriginEndpoints](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 