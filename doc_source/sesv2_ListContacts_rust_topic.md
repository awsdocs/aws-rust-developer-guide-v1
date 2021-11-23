--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List the contacts in a contact list<a name="sesv2_ListContacts_rust_topic"></a>

The following code example shows how to list the contacts in an Amazon SES API v2 contact list\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_contacts(client: &Client, list: &str) -> Result<(), Error> {
    let resp = client
        .list_contacts()
        .contact_list_name(list)
        .send()
        .await?;

    println!("Contacts:");

    for contact in resp.contacts.unwrap_or_default() {
        println!("  {}", contact.email_address.as_deref().unwrap_or_default());
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ses#code-examples)\. 
+  For API details, see [ListContacts](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 