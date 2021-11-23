--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Describes scaling policies<a name="application-autoscaling_DescribeScalingPolicies_rust_topic"></a>

The following code example shows how to describe Application Auto Scaling scaling policies for the specified service namespace\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_policies(client: &Client) -> Result<(), Error> {
    let response = client
        .describe_scaling_policies()
        .service_namespace(ServiceNamespace::Ec2)
        .send()
        .await?;
    if let Some(policies) = response.scaling_policies() {
        println!("Auto Scaling Policies:");
        for policy in policies {
            println!("{:?}\n", policy);
        }
    }
    println!("Next token: {:?}", response.next_token());

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/applicationautoscaling#code-examples)\. 
+  For API details, see [DescribeScalingPolicies](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 