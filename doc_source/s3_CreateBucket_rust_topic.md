--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Create a bucket<a name="s3_CreateBucket_rust_topic"></a>

The following code example shows how to create an Amazon S3 bucket\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn make_bucket(client: &Client, bucket: &str, region: &str) -> Result<(), Error> {
    let constraint = BucketLocationConstraint::from(region);
    let cfg = CreateBucketConfiguration::builder()
        .location_constraint(constraint)
        .build();

    client
        .create_bucket()
        .create_bucket_configuration(cfg)
        .bucket(bucket)
        .send()
        .await?;
    println!("Created bucket.");

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/s3#code-examples)\. 
+  For API details, see [CreateBucket](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 