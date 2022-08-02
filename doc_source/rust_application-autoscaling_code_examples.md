--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Application Auto Scaling examples using SDK for Rust<a name="rust_application-autoscaling_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Application Auto Scaling\.

*Actions* are code excerpts that show you how to call individual Application Auto Scaling functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Application Auto Scaling functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c15c13)

## Actions<a name="w14aac14b9c15c13"></a>

### Describes scaling policies<a name="application-autoscaling_DescribeScalingPolicies_rust_topic"></a>

The following code example shows how to describe Application Auto Scaling scaling policies for the specified service namespace\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/applicationautoscaling#code-examples)\. 
  

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
+  For API details, see [DescribeScalingPolicies](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 