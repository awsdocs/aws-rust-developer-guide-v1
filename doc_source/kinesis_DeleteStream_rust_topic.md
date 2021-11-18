--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Delete a stream<a name="kinesis_DeleteStream_rust_topic"></a>

The following code example shows how to delete a Kinesis stream\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn remove_stream(client: &Client, stream: &str) -> Result<(), Error> {
    client.delete_stream().stream_name(stream).send().await?;

    println!("Deleted stream.");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/kinesis#code-examples)\. 
+  For API details, see [DeleteStream](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 