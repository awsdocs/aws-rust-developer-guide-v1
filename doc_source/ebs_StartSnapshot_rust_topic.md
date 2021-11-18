--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Create a snapshot<a name="ebs_StartSnapshot_rust_topic"></a>

The following code example shows how to create an Amazon EBS snapshot\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn start(client: &Client, description: &str) -> Result<String, Error> {
    let snapshot = client
        .start_snapshot()
        .description(description)
        .encrypted(false)
        .volume_size(1)
        .send()
        .await?;

    Ok(snapshot.snapshot_id.unwrap())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/ebs#code-examples)\. 
+  For API details, see [StartSnapshot](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 