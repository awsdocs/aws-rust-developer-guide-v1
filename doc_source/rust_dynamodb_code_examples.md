--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# DynamoDB examples using SDK for Rust<a name="rust_dynamodb_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with DynamoDB\.

*Actions* are code excerpts that show you how to call individual DynamoDB functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple DynamoDB functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c23c13)

## Actions<a name="w14aac14b9c23c13"></a>

### Create a table<a name="dynamodb_CreateTable_rust_topic"></a>

The following code example shows how to create a DynamoDB table\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/dynamodb#code-examples)\. 
  

```rust
async fn create_table(client: &Client, table: &str, key: &str) -> Result<(), Error> {
    let a_name: String = key.into();
    let table_name: String = table.into();

    let ad = AttributeDefinition::builder()
        .attribute_name(&a_name)
        .attribute_type(ScalarAttributeType::S)
        .build();

    let ks = KeySchemaElement::builder()
        .attribute_name(&a_name)
        .key_type(KeyType::Hash)
        .build();

    let pt = ProvisionedThroughput::builder()
        .read_capacity_units(10)
        .write_capacity_units(5)
        .build();

    match client
        .create_table()
        .table_name(table_name)
        .key_schema(ks)
        .attribute_definitions(ad)
        .provisioned_throughput(pt)
        .send()
        .await
    {
        Ok(_) => println!("Added table {} with key {}", table, key),
        Err(e) => {
            println!("Got an error creating table:");
            println!("{}", e);
            process::exit(1);
        }
    };

    Ok(())
}
```
+  For API details, see [CreateTable](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Delete a table<a name="dynamodb_DeleteTable_rust_topic"></a>

The following code example shows how to delete a DynamoDB table\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/dynamodb#code-examples)\. 
  

```rust
async fn delete_table(client: &Client, table: &str) -> Result<(), Error> {
    client.delete_table().table_name(table).send().await?;

    println!("Deleted table");

    Ok(())
}
```
+  For API details, see [DeleteTable](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Delete an item from a table<a name="dynamodb_DeleteItem_rust_topic"></a>

The following code example shows how to delete an item from a DynamoDB table\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/dynamodb#code-examples)\. 
  

```rust
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
+  For API details, see [DeleteItem](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### List tables<a name="dynamodb_ListTables_rust_topic"></a>

The following code example shows how to list DynamoDB tables\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/dynamodb#code-examples)\. 
  

```rust
async fn list_tables(client: &Client) -> Result<(), Error> {
    let paginator = client.list_tables().into_paginator().items().send();
    let table_names = paginator.collect::<Result<Vec<_>, _>>().await?;

    println!("Tables:");

    for name in &table_names {
        println!("  {}", name);
    }

    println!("Found {} tables", table_names.len());
    Ok(())
}
```
Determine whether table exists\.  

```rust
async fn does_table_exist(client: &Client, table: &str) -> Result<bool, Error> {
    let table_exists = client
        .list_tables()
        .send()
        .await
        .expect("should succeed")
        .table_names()
        .as_ref()
        .unwrap()
        .contains(&table.into());

    Ok(table_exists)
}
```
+  For API details, see [ListTables](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Put an item in a table<a name="dynamodb_PutItem_rust_topic"></a>

The following code example shows how to put an item in a DynamoDB table\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/dynamodb#code-examples)\. 
  

```rust
async fn add_item(
    client: &Client,
    table: &str,
    username: &str,
    p_type: &str,
    age: &str,
    first: &str,
    last: &str,
) -> Result<(), Error> {
    let user_av = AttributeValue::S(username.into());
    let type_av = AttributeValue::S(p_type.into());
    let age_av = AttributeValue::S(age.into());
    let first_av = AttributeValue::S(first.into());
    let last_av = AttributeValue::S(last.into());

    let request = client
        .put_item()
        .table_name(table)
        .item("username", user_av)
        .item("account_type", type_av)
        .item("age", age_av)
        .item("first_name", first_av)
        .item("last_name", last_av);

    println!("Executing request [{:?}] to add item...", request);

    request.send().await?;

    println!(
        "Added user {}, {} {}, age {} as {} user",
        username, first, last, age, p_type
    );

    Ok(())
}
```
+  For API details, see [PutItem](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Query a table<a name="dynamodb_Query_rust_topic"></a>

The following code example shows how to query a DynamoDB table\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/dynamodb#code-examples)\. 
Find the movies made in the specified year\.  

```rust
fn movies_in_year(client: &Client, table_name: &str, year: u16) -> Query {
    client
        .query()
        .table_name(table_name)
        .key_condition_expression("#yr = :yyyy")
        .expression_attribute_names("#yr", "year")
        .expression_attribute_values(":yyyy", AttributeValue::N(year.to_string()))
}
```
+  For API details, see [Query](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Scan a table<a name="dynamodb_Scan_rust_topic"></a>

The following code example shows how to scan a DynamoDB table\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/dynamodb#code-examples)\. 
  

```rust
async fn list_items(client: &Client, table: &str) -> Result<(), Error> {
    let items: Result<Vec<_>, _> = client
        .scan()
        .table_name(table)
        .into_paginator()
        .items()
        .send()
        .collect()
        .await;

    println!("Items in table:");
    for item in items? {
        println!("   {:?}", item);
    }

    Ok(())
}
```
+  For API details, see [Scan](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 