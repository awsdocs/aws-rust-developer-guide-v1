--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Put data into a stream<a name="kinesis_PutRecord_rust_topic"></a>

The following code example shows how to put data into a Kinesis stream\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn add_record(client: &Client, stream: &str, key: &str, data: &str) -> Result<(), Error> {
    let blob = Blob::new(data);

    client
        .put_record()
        .data(blob)
        .partition_key(key)
        .stream_name(stream)
        .send()
        .await?;

    println!("Put data into stream.");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/kinesis#code-examples)\. 
+  For API details, see [PutRecord](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 