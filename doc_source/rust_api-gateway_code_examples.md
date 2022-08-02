--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# API Gateway examples using SDK for Rust<a name="rust_api-gateway_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with API Gateway\.

*Actions* are code excerpts that show you how to call individual API Gateway functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple API Gateway functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c11c13)

## Actions<a name="w14aac14b9c11c13"></a>

### List REST APIs<a name="api-gateway_GetRestApis_rust_topic"></a>

The following code example shows how to list API Gateway REST APIs\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/apigateway#code-examples)\. 
Displays the Amazon API Gateway REST APIs in the Region\.  

```
async fn show_apis(client: &Client) -> Result<(), Error> {
    let resp = client.get_rest_apis().send().await?;

    for api in resp.items().unwrap_or_default() {
        println!("ID:          {}", api.id().unwrap_or_default());
        println!("Name:        {}", api.name().unwrap_or_default());
        println!("Description: {}", api.description().unwrap_or_default());
        println!("Version:     {}", api.version().unwrap_or_default());
        println!(
            "Created:     {}",
            api.created_date().unwrap().to_chrono_utc()
        );
        println!();
    }

    Ok(())
}
```
+  For API details, see [GetRestApis](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 