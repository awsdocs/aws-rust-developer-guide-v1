--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Stop an instance<a name="ec2_StopInstances_rust_topic"></a>

The following code example shows how to stop an Amazon EC2 instance\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn stop_instance(client: &Client, id: &str) -> Result<(), Error> {
    client.stop_instances().instance_ids(id).send().await?;

    println!("Stopped instance.");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/ec2#code-examples)\. 
+  For API details, see [StopInstances](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 