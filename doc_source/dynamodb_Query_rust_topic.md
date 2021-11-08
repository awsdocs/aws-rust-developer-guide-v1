--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Query a table<a name="dynamodb_Query_rust_topic"></a>

The following code example shows how to query a DynamoDB table\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
Find the movies made in the specified year\.  

```
fn movies_in_year(client: &Client, table_name: &str, year: u16) -> Query {
    client
        .query()
        .table_name(table_name)
        .key_condition_expression("#yr = :yyyy")
        .expression_attribute_names("#yr", "year")
        .expression_attribute_values(":yyyy", AttributeValue::N(year.to_string()))
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/dynamodb#code-examples)\. 
+  For API details, see [Query](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 