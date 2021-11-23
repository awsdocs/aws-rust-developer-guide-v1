--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Get identity information<a name="sesv2_GetEmailIdentity_rust_topic"></a>

The following code example shows how to get Amazon SES API v2 identity information\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
Determines whether an email address has been verified\.  

```
async fn is_verified(client: &Client, email: &str) -> Result<(), Error> {
    let resp = client
        .get_email_identity()
        .email_identity(email)
        .send()
        .await?;

    if resp.verified_for_sending_status {
        println!("The address is verified");
    } else {
        println!("The address is not verified");
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ses#code-examples)\. 
+  For API details, see [GetEmailIdentity](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 