--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Delete a snapshot<a name="ec2_DeleteSnapshot_rust_topic"></a>

The following code example shows how to delete an Amazon EBS snapshot\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn delete_snapshot(client: &Client, id: &str) -> Result<(), Error> {
    client.delete_snapshot().snapshot_id(id).send().await?;

    println!("Deleted");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ebs#code-examples)\. 
+  For API details, see [DeleteSnapshot](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 