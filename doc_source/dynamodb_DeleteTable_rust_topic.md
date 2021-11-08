--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Delete a table<a name="dynamodb_DeleteTable_rust_topic"></a>

The following code example shows how to delete a DynamoDB table\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn delete_table(client: &Client, table: &str) -> Result<(), Error> {
    client.delete_table().table_name(table).send().await?;

    println!("Deleted table");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/dynamodb#code-examples)\. 
+  For API details, see [DeleteTable](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 