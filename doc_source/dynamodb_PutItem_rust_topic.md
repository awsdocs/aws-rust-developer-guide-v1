--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Put an item in a table<a name="dynamodb_PutItem_rust_topic"></a>

The following code example shows how to put an item in a DynamoDB table\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/dynamodb#code-examples)\. 
+  For API details, see [PutItem](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 