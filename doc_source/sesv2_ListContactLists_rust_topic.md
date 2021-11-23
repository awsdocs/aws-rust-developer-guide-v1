--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List the contact lists<a name="sesv2_ListContactLists_rust_topic"></a>

The following code example shows how to list the Amazon SES API v2 contact lists\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_lists(client: &Client) -> Result<(), Error> {
    let resp = client.list_contact_lists().send().await?;

    println!("Contact lists:");

    for list in resp.contact_lists.unwrap_or_default() {
        println!(
            "  {}",
            list.contact_list_name.as_deref().unwrap_or_default()
        );
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ses#code-examples)\. 
+  For API details, see [ListContactLists](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 