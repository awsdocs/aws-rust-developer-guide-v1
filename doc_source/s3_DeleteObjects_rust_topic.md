--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Delete multiple objects<a name="s3_DeleteObjects_rust_topic"></a>

The following code example shows how to delete multiple objects from an Amazon S3 bucket\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn remove_objects(client: &Client, bucket: &str, objects: Vec<String>) -> Result<(), Error> {
    let mut delete_objects: Vec<ObjectIdentifier> = vec![];

    for obj in objects {
        let obj_id = ObjectIdentifier::builder().set_key(Some(obj)).build();
        delete_objects.push(obj_id);
    }

    let delete = Delete::builder().set_objects(Some(delete_objects)).build();

    client
        .delete_objects()
        .bucket(bucket)
        .delete(delete)
        .send()
        .await?;

    println!("Objects deleted.");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/S3#code-examples)\. 
+  For API details, see [DeleteObjects](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 