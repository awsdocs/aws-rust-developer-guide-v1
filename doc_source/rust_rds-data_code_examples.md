--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Amazon RDS Data Service examples using SDK for Rust<a name="rust_rds-data_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Amazon RDS Data Service\.

*Actions* are code excerpts that show you how to call individual Amazon RDS Data Service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Amazon RDS Data Service functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c53c13)

## Actions<a name="w14aac14b9c53c13"></a>

### Run an SQL statement<a name="rds-data_ExecuteStatement_rust_topic"></a>

The following code example shows how to run an SQL statement\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn query_cluster(
    client: &Client,
    cluster_arn: &str,
    query: &str,
    secret_arn: &str,
) -> Result<(), Error> {
    let st = client
        .execute_statement()
        .resource_arn(cluster_arn)
        .database("postgres") // Do not confuse this with db instance name
        .sql(query)
        .secret_arn(secret_arn);

    let result = st.send().await?;

    println!("{:?}", result);
    println!();

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/rdsdata#code-examples)\. 
+  For API details, see [ExecuteStatement](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 