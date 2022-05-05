--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# QLDB examples using SDK for Rust<a name="rust_qldb_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with QLDB\.

*Actions* are code excerpts that show you how to call individual QLDB functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple QLDB functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c51c13)

## Actions<a name="w14aac14b9c51c13"></a>

### Create a ledger<a name="qldb_CreateLedger_rust_topic"></a>

The following code example shows how to create a QLDB ledger\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn make_ledger(client: &Client, ledger: &str) -> Result<(), Error> {
    let result = client
        .create_ledger()
        .name(ledger)
        .permissions_mode(PermissionsMode::AllowAll)
        .send()
        .await?;

    println!("ARN: {}", result.arn().unwrap());

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/qldb#code-examples)\. 
+  For API details, see [CreateLedger](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### List your ledgers<a name="qldb_ListLedgers_rust_topic"></a>

The following code example shows how to list your QLDB ledgers\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_ledgers(client: &Client) -> Result<(), Error> {
    let result = client.list_ledgers().send().await?;

    if let Some(ledgers) = result.ledgers() {
        for ledger in ledgers {
            println!("* {:?}", ledger);
        }

        if result.next_token().is_some() {
            todo!("pagination is not yet demonstrated")
        }
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/qldb#code-examples)\. 
+  For API details, see [ListLedgers](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 