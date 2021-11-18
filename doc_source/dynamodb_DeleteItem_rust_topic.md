--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Delete an item from a table<a name="dynamodb_DeleteItem_rust_topic"></a>

The following code example shows how to delete an item from a DynamoDB table\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn delete_item(client: &Client, table: &str, key: &str, value: &str) -> Result<(), Error> {
    match client
        .delete_item()
        .table_name(table)
        .key(key, AttributeValue::S(value.into()))
        .send()
        .await
    {
        Ok(_) => println!("Deleted item from table"),
        Err(e) => {
            println!("Got an error deleting item from table:");
            println!("{}", e);
            process::exit(1);
        }
    };

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/dynamodb#code-examples)\. 
+  For API details, see [DeleteItem](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 