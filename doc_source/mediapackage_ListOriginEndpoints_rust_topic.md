--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List your origin endpoints<a name="mediapackage_ListOriginEndpoints_rust_topic"></a>

The following code example shows how to list your MediaPackage origin endpoints\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
List your endpoint descriptions and URLs\.  

```
async fn show_endpoints(client: &Client) -> Result<(), Error> {
    let or_endpoints = client.list_origin_endpoints().send().await?;

    println!("Endpoints:");

    for e in or_endpoints.origin_endpoints.unwrap_or_default() {
        let endpoint_url = e.url.as_deref().unwrap_or_default();
        let endpoint_description = e.description.as_deref().unwrap_or_default();
        println!("  Description: {}", endpoint_description);
        println!("  URL :        {}", endpoint_url);
        println!();
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/mediapackage#code-examples)\. 
+  For API details, see [ListOriginEndpoints](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 