--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List REST APIs<a name="api-gateway_GetRestApis_rust_topic"></a>

The following code example shows how to list API Gateway REST APIs\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
Displays the Amazon API Gateway REST APIs in the Region\.  

```
async fn show_apis(client: &Client) -> Result<(), Error> {
    let resp = client.get_rest_apis().send().await?;

    for api in resp.items.unwrap_or_default() {
        println!("ID:          {}", api.id.as_deref().unwrap_or_default());
        println!("Name:        {}", api.name.as_deref().unwrap_or_default());
        println!(
            "Description: {}",
            api.description.as_deref().unwrap_or_default()
        );
        println!(
            "Version:     {}",
            api.version.as_deref().unwrap_or_default()
        );
        println!("Created:     {}", api.created_date.unwrap().to_chrono());
        println!();
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/apigateway#code-examples)\. 
+  For API details, see [GetRestApis](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 