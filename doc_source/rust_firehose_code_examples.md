--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Kinesis Data Firehose examples using SDK for Rust<a name="rust_firehose_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Kinesis Data Firehose\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)

## Actions<a name="actions"></a>

### Write multiple data records<a name="firehose_PutRecordBatch_rust_topic"></a>

The following code example shows how to write multiple data records into a Kinesis Data Firehose delivery stream in a single call, which can achieve higher throughput per producer than when writing single records\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/firehose#code-examples)\. 
  

```
async fn put_record_batch(
    client: &Client,
    stream: &str,
    data: Option<Vec<Record>>,
) -> Result<PutRecordBatchOutput, SdkError<PutRecordBatchError>> {
    client
        .put_record_batch()
        .delivery_stream_name(stream)
        .set_records(data)
        .send()
        .await
}
```
+  For API details, see [PutRecordBatch](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 