--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Create a stream<a name="kinesis_CreateStream_rust_topic"></a>

The following code example shows how to create a Kinesis stream\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn make_stream(client: &Client, stream: &str) -> Result<(), Error> {
    client
        .create_stream()
        .stream_name(stream)
        .shard_count(4)
        .send()
        .await?;

    println!("Created stream");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/kinesis#code-examples)\. 
+  For API details, see [CreateStream](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 