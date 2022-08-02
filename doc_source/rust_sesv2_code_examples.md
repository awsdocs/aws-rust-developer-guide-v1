--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Amazon SES API v2 examples using SDK for Rust<a name="rust_sesv2_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Amazon SES API v2\.

*Actions* are code excerpts that show you how to call individual Amazon SES API v2 functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Amazon SES API v2 functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c59c13)

## Actions<a name="w14aac14b9c59c13"></a>

### Create a contact in a contact list<a name="sesv2_CreateContact_rust_topic"></a>

The following code example shows how to create an Amazon SES API v2 contact in a contact list\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ses#code-examples)\. 
  

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
+  For API details, see [CreateContact](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Create a contact list<a name="sesv2_CreateContactList_rust_topic"></a>

The following code example shows how to create an Amazon SES API v2 contact list\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ses#code-examples)\. 
  

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
+  For API details, see [CreateContactList](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Get identity information<a name="sesv2_GetEmailIdentity_rust_topic"></a>

The following code example shows how to get Amazon SES API v2 identity information\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ses#code-examples)\. 
Determines whether an email address has been verified\.  

```
async fn is_verified(client: &Client, email: &str) -> Result<(), Error> {
    let resp = client
        .get_email_identity()
        .email_identity(email)
        .send()
        .await?;

    if resp.verified_for_sending_status() {
        println!("The address is verified");
    } else {
        println!("The address is not verified");
    }

    Ok(())
}
```
+  For API details, see [GetEmailIdentity](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### List the contact lists<a name="sesv2_ListContactLists_rust_topic"></a>

The following code example shows how to list the Amazon SES API v2 contact lists\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ses#code-examples)\. 
  

```
async fn show_lists(client: &Client) -> Result<(), Error> {
    let resp = client.list_contact_lists().send().await?;

    println!("Contact lists:");

    for list in resp.contact_lists().unwrap_or_default() {
        println!("  {}", list.contact_list_name().unwrap_or_default());
    }

    Ok(())
}
```
+  For API details, see [ListContactLists](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### List the contacts in a contact list<a name="sesv2_ListContacts_rust_topic"></a>

The following code example shows how to list the contacts in an Amazon SES API v2 contact list\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ses#code-examples)\. 
  

```
async fn show_contacts(client: &Client, list: &str) -> Result<(), Error> {
    let resp = client
        .list_contacts()
        .contact_list_name(list)
        .send()
        .await?;

    println!("Contacts:");

    for contact in resp.contacts().unwrap_or_default() {
        println!("  {}", contact.email_address().unwrap_or_default());
    }

    Ok(())
}
```
+  For API details, see [ListContacts](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Send an email<a name="sesv2_SendEmail_rust_topic"></a>

The following code example shows how to send an Amazon SES API v2 email\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ses#code-examples)\. 
Sends a message to all members of the contact list\.  

```
async fn send_message(
    client: &Client,
    list: &str,
    from: &str,
    subject: &str,
    message: &str,
) -> Result<(), Error> {
    // Get list of email addresses from contact list.
    let resp = client
        .list_contacts()
        .contact_list_name(list)
        .send()
        .await?;

    let contacts = resp.contacts().unwrap_or_default();

    let cs: String = contacts
        .iter()
        .map(|i| i.email_address().unwrap_or_default())
        .collect();

    let dest = Destination::builder().to_addresses(cs).build();
    let subject_content = Content::builder().data(subject).charset("UTF-8").build();
    let body_content = Content::builder().data(message).charset("UTF-8").build();
    let body = Body::builder().text(body_content).build();

    let msg = Message::builder()
        .subject(subject_content)
        .body(body)
        .build();

    let email_content = EmailContent::builder().simple(msg).build();

    client
        .send_email()
        .from_email_address(from)
        .destination(dest)
        .content(email_content)
        .send()
        .await?;

    println!("Email sent to list");

    Ok(())
}
```
+  For API details, see [SendEmail](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 