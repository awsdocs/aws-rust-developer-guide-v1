--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List tables<a name="dynamodb_ListTables_rust_topic"></a>

The following code example shows how to list DynamoDB tables\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn list_tables(client: &Client) -> Result<(), Error> {
    let resp = client.list_tables().send().await?;

    println!("Tables:");

    let names = resp.table_names.unwrap_or_default();
    let len = names.len();

    for name in names {
        println!("  {}", name);
    }

    println!("Found {} tables", len);

    Ok(())
}
```
Determine whether table exists\.  

```
async fn does_table_exist(client: &Client, table: &str) -> Result<bool, Error> {
    let table_exists = client
        .list_tables()
        .send()
        .await
        .expect("should succeed")
        .table_names
        .as_ref()
        .unwrap()
        .contains(&table.into());

    Ok(table_exists)
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/dynamodb#code-examples)\. 
+  For API details, see [ListTables](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 