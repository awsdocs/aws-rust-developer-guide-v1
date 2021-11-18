--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List objects in a bucket<a name="s3_ListObjects_rust_topic"></a>

The following code example shows how to list objects located in an Amazon S3 bucket\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_objects(client: &Client, bucket: &str) -> Result<(), Error> {
    let resp = client.list_objects_v2().bucket(bucket).send().await?;

    for object in resp.contents.unwrap_or_default() {
        println!("{}", object.key.as_deref().unwrap_or_default());
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/s3#code-examples)\. 
+  For API details, see [ListObjects](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 