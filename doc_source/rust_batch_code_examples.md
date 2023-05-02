--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# AWS Batch examples using SDK for Rust<a name="rust_batch_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with AWS Batch\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)

## Actions<a name="actions"></a>

### Describe compute environments<a name="batch_DescribeComputeEnvironments_rust_topic"></a>

The following code example shows how to describe one or more AWS Batch compute environments\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/batch#code-examples)\. 
  

```
async fn show_envs(client: &Client) -> Result<(), Error> {
    let rsp = client.describe_compute_environments().send().await?;

    let compute_envs = rsp.compute_environments().unwrap_or_default();
    println!("Found {} compute environments:", compute_envs.len());
    for env in compute_envs {
        let arn = env.compute_environment_arn().unwrap_or_default();
        let name = env.compute_environment_name().unwrap_or_default();

        println!("  Name : {}", name);
        println!("  ARN:   {}", arn);
        println!();
    }

    Ok(())
}
```
+  For API details, see [DescribeComputeEnvironments](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 