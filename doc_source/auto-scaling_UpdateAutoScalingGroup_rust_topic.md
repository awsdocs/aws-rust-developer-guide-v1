--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Update group configuration<a name="auto-scaling_UpdateAutoScalingGroup_rust_topic"></a>

The following code example shows how to update the configuration for an Auto Scaling group\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn update_group(client: &Client, name: &str, size: i32) -> Result<(), Error> {
    client
        .update_auto_scaling_group()
        .auto_scaling_group_name(name)
        .max_size(size)
        .send()
        .await?;

    println!("Updated AutoScaling group");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/autoscaling#code-examples)\. 
+  For API details, see [UpdateAutoScalingGroup](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 