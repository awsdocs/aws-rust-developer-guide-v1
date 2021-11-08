--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Delete an object<a name="s3_DeleteObject_rust_topic"></a>

The following code example shows how to delete an Amazon S3 object\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn remove_object(client: &Client, bucket: &str, key: &str) -> Result<(), Error> {
    client
        .delete_object()
        .bucket(bucket)
        .key(key)
        .send()
        .await?;

    println!("Object deleted.");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/s3#code-examples)\. 
+  For API details, see [DeleteObject](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 