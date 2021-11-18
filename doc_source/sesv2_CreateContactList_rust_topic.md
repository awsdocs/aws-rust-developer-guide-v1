--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Create a contact list<a name="sesv2_CreateContactList_rust_topic"></a>

The following code example shows how to create an Amazon SES API v2 contact list\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn make_list(client: &Client, contact_list: &str) -> Result<(), Error> {
    client
        .create_contact_list()
        .contact_list_name(contact_list)
        .send()
        .await?;

    println!("Created contact list.");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/ses#code-examples)\. 
+  For API details, see [CreateContactList](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 