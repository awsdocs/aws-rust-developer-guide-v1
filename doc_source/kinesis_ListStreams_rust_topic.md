--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List streams<a name="kinesis_ListStreams_rust_topic"></a>

The following code example shows how to list information about one or more Kinesis streams\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_streams(client: &Client) -> Result<(), Error> {
    let resp = client.list_streams().send().await?;

    println!("Stream names:");

    let streams = resp.stream_names.unwrap_or_default();
    for stream in &streams {
        println!("  {}", stream);
    }

    println!("Found {} stream(s)", streams.len());

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/kinesis#code-examples)\. 
+  For API details, see [ListStreams](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 