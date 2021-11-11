--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List your ledgers<a name="qldb_ListLedgers_rust_topic"></a>

The following code example shows how to list your QLDB ledgers\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_ledgers(client: &Client) -> Result<(), Error> {
    let result = client.list_ledgers().send().await?;

    if let Some(ledgers) = result.ledgers {
        for ledger in ledgers {
            println!("* {:?}", ledger);
        }

        if result.next_token.is_some() {
            todo!("pagination is not yet demonstrated")
        }
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/qldb#code-examples)\. 
+  For API details, see [ListLedgers](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 