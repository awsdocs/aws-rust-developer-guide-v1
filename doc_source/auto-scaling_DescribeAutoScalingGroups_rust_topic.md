--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Get group information<a name="auto-scaling_DescribeAutoScalingGroups_rust_topic"></a>

The following code example shows how to get information about Auto Scaling groups\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn list_groups(client: &Client) -> Result<(), Error> {
    let resp = client.describe_auto_scaling_groups().send().await?;

    println!("Groups:");

    let groups = resp.auto_scaling_groups.unwrap_or_default();

    for group in &groups {
        println!(
            "  {}",
            group.auto_scaling_group_name.as_deref().unwrap_or_default()
        );
        println!(
            "  ARN:          {}",
            group.auto_scaling_group_arn.as_deref().unwrap_or_default()
        );
        println!("  Minimum size: {}", group.min_size.unwrap_or_default());
        println!("  Maximum size: {}", group.max_size.unwrap_or_default());
        println!();
    }

    println!("Found {} group(s)", groups.len());

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/autoscaling#code-examples)\. 
+  For API details, see [DescribeAutoScalingGroups](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 