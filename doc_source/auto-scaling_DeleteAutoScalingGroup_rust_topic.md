--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Delete a group<a name="auto-scaling_DeleteAutoScalingGroup_rust_topic"></a>

The following code example shows how to delete an Auto Scaling group\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn delete_group(client: &Client, name: &str, force: bool) -> Result<(), Error> {
    client
        .delete_auto_scaling_group()
        .auto_scaling_group_name(name)
        .set_force_delete(force.then(|| true))
        .send()
        .await?;

    println!("Deleted Auto Scaling group");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/autoscaling#code-examples)\. 
+  For API details, see [DeleteAutoScalingGroup](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 