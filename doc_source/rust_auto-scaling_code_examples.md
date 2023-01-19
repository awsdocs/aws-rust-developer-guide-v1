--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Amazon EC2 Auto Scaling examples using SDK for Rust<a name="rust_auto-scaling_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Amazon EC2 Auto Scaling\.

*Actions* are code excerpts that show you how to call individual Amazon EC2 Auto Scaling functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Amazon EC2 Auto Scaling functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c29c13)

## Actions<a name="w14aac14b9c29c13"></a>

### Create a group<a name="auto-scaling_CreateAutoScalingGroup_rust_topic"></a>

The following code example shows how to create an Auto Scaling group\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/autoscaling#code-examples)\. 
  

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
+  For API details, see [CreateAutoScalingGroup](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Delete a group<a name="auto-scaling_DeleteAutoScalingGroup_rust_topic"></a>

The following code example shows how to delete an Auto Scaling group\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/autoscaling#code-examples)\. 
  

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
+  For API details, see [DeleteAutoScalingGroup](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Get information about groups<a name="auto-scaling_DescribeAutoScalingGroups_rust_topic"></a>

The following code example shows how to get information about Auto Scaling groups\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/autoscaling#code-examples)\. 
  

```
async fn list_groups(client: &Client) -> Result<(), Error> {
    let resp = client.describe_auto_scaling_groups().send().await?;

    println!("Groups:");

    let groups = resp.auto_scaling_groups().unwrap_or_default();

    for group in groups {
        println!("  {}", group.auto_scaling_group_name().unwrap_or_default());
        println!(
            "  ARN:          {}",
            group.auto_scaling_group_arn().unwrap_or_default()
        );
        println!("  Minimum size: {}", group.min_size().unwrap_or_default());
        println!("  Maximum size: {}", group.max_size().unwrap_or_default());
        println!();
    }

    println!("Found {} group(s)", groups.len());

    Ok(())
}
```
+  For API details, see [DescribeAutoScalingGroups](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Update a group<a name="auto-scaling_UpdateAutoScalingGroup_rust_topic"></a>

The following code example shows how to update the configuration for an Auto Scaling group\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/autoscaling#code-examples)\. 
  

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
+  For API details, see [UpdateAutoScalingGroup](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 