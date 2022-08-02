--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Amazon EBS examples using SDK for Rust<a name="rust_ebs_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Amazon EBS\.

*Actions* are code excerpts that show you how to call individual Amazon EBS functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Amazon EBS functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c25c13)

## Actions<a name="w14aac14b9c25c13"></a>

### Create a snapshot<a name="ebs_StartSnapshot_rust_topic"></a>

The following code example shows how to create an Amazon EBS snapshot\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ebs#code-examples)\. 
  

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
+  For API details, see [StartSnapshot](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Seal and complete a snapshot<a name="ebs_CompleteSnapshot_rust_topic"></a>

The following code example shows how to seal and complete an Amazon EBS snapshot\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ebs#code-examples)\. 
  

```
async fn finish(client: &Client, id: &str) -> Result<(), Error> {
    client
        .complete_snapshot()
        .changed_blocks_count(2)
        .snapshot_id(id)
        .send()
        .await?;

    println!("Snapshot ID {}", id);
    println!("The state is 'completed' when all of the modified blocks have been transferred to Amazon S3.");
    println!("Use the get-snapshot-state code example to get the state of the snapshot.");

    Ok(())
}
```
+  For API details, see [CompleteSnapshot](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Write a block of data to a snapshot<a name="ebs_PutSnapshotBlock_rust_topic"></a>

The following code example shows how to write a block of data to an Amazon EBS snapshot\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ebs#code-examples)\. 
  

```
async fn add_block(
    client: &Client,
    id: &str,
    idx: usize,
    block: Vec<u8>,
    checksum: &str,
) -> Result<(), Error> {
    client
        .put_snapshot_block()
        .snapshot_id(id)
        .block_index(idx as i32)
        .block_data(ByteStream::from(block))
        .checksum(checksum)
        .checksum_algorithm(ChecksumAlgorithm::ChecksumAlgorithmSha256)
        .data_length(EBS_BLOCK_SIZE as i32)
        .send()
        .await?;

    Ok(())
}
```
+  For API details, see [PutSnapshotBlock](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 