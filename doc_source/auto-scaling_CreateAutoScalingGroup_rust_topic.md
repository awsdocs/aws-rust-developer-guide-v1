--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Create a group<a name="auto-scaling_CreateAutoScalingGroup_rust_topic"></a>

The following code example shows how to create an Auto Scaling group\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn create_group(client: &Client, name: &str, id: &str) -> Result<(), Error> {
    client
        .create_auto_scaling_group()
        .auto_scaling_group_name(name)
        .instance_id(id)
        .min_size(1)
        .max_size(5)
        .send()
        .await?;

    println!("Created AutoScaling group");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/autoscaling#code-examples)\. 
+  For API details, see [CreateAutoScalingGroup](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 