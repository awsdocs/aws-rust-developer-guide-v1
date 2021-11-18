--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Start an instance<a name="ec2_StartInstances_rust_topic"></a>

The following code example shows how to start an Amazon EC2 instance\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn start_instance(client: &Client, id: &str) -> Result<(), Error> {
    client.start_instances().instance_ids(id).send().await?;

    println!("Started instance.");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/ec2#code-examples)\. 
+  For API details, see [StartInstances](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 