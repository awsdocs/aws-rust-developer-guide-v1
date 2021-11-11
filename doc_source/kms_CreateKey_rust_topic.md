--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Create a KMS key<a name="kms_CreateKey_rust_topic"></a>

The following code example shows how to create a KMS key\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn make_key(client: &Client) -> Result<(), Error> {
    let resp = client.create_key().send().await?;

    let id = resp
        .key_metadata
        .unwrap()
        .key_id
        .unwrap_or_else(|| String::from("No ID!"));

    println!("Key: {}", id);

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/kms#code-examples)\. 
+  For API details, see [CreateKey](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 