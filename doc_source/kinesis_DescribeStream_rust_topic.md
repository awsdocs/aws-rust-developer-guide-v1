--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Describe a stream<a name="kinesis_DescribeStream_rust_topic"></a>

The following code example shows how to describe a Kinesis stream\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_stream(client: &Client, stream: &str) -> Result<(), Error> {
    let resp = client.describe_stream().stream_name(stream).send().await?;

    let desc = resp.stream_description.unwrap();

    println!("Stream description:");
    println!("  Name:              {}:", desc.stream_name.unwrap());
    println!("  Status:            {:?}", desc.stream_status.unwrap());
    println!("  Open shards:       {:?}", desc.shards.unwrap().len());
    println!(
        "  Retention (hours): {}",
        desc.retention_period_hours.unwrap()
    );
    println!("  Encryption:        {:?}", desc.encryption_type.unwrap());

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/kinesis#code-examples)\. 
+  For API details, see [DescribeStream](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 