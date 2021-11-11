--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Create a contact in a contact list<a name="sesv2_CreateContact_rust_topic"></a>

The following code example shows how to create an Amazon SES API v2 contact in a contact list\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn add_contact(client: &Client, list: &str, email: &str) -> Result<(), Error> {
    client
        .create_contact()
        .contact_list_name(list)
        .email_address(email)
        .send()
        .await?;

    println!("Created contact");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/ses#code-examples)\. 
+  For API details, see [CreateContact](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 