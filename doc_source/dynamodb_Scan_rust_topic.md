--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Scan a table<a name="dynamodb_Scan_rust_topic"></a>

The following code example shows how to scan a DynamoDB table\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn list_items(client: &Client, table: &str) -> Result<(), Error> {
    let resp = client.scan().table_name(table).send().await?;

    println!("Items in table:");

    if let Some(item) = resp.items {
        println!("   {:?}", item);
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/dynamodb#code-examples)\. 
+  For API details, see [Scan](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 