--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# API Gateway Management API examples using SDK for Rust<a name="rust_apigatewaymanagementapi_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with API Gateway Management API\.

*Actions* are code excerpts that show you how to call individual API Gateway Management API functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple API Gateway Management API functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c13c13)

## Actions<a name="w14aac14b9c13c13"></a>

### Send data to a connection<a name="apigatewaymanagementapi_PostToConnection_rust_topic"></a>

The following code example shows how to send data to a connection\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/apigatewaymanagement#code-examples)\. 
  

```rust
async fn send_data(
    client: &aws_sdk_apigatewaymanagement::Client,
    con_id: &str,
    data: &str,
) -> Result<(), aws_sdk_apigatewaymanagement::Error> {
    client
        .post_to_connection()
        .connection_id(con_id)
        .data(Blob::new(data))
        .send()
        .await?;

    Ok(())
}

    let uri = format!(
        "https://{api_id}.execute-api.{region}.amazonaws.com/{stage}",
        api_id = api_id,
        region = region,
        stage = stage
    )
    .parse()
    .expect("could not construct valid URI for endpoint");
    let endpoint = Endpoint::immutable(uri);

    let shared_config = aws_config::from_env().region(region_provider).load().await;
    let api_management_config = config::Builder::from(&shared_config)
        .endpoint_resolver(endpoint)
        .build();
    let client = Client::from_conf(api_management_config);
```
+  For API details, see [PostToConnection](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 