--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List buckets<a name="s3_ListBuckets_rust_topic"></a>

The following code example shows how to list Amazon S3 buckets\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_buckets(strict: bool, client: &Client, region: &str) -> Result<(), Error> {
    let resp = client.list_buckets().send().await?;
    let buckets = resp.buckets.unwrap_or_default();
    let num_buckets = buckets.len();

    let mut in_region = 0;

    for bucket in &buckets {
        if strict {
            let r = client
                .get_bucket_location()
                .bucket(bucket.name.as_deref().unwrap_or_default())
                .send()
                .await?;

            if r.location_constraint.unwrap().as_ref() == region {
                println!("{}", bucket.name.as_deref().unwrap_or_default());
                in_region += 1;
            }
        } else {
            println!("{}", bucket.name.as_deref().unwrap_or_default());
        }
    }

    println!();
    if strict {
        println!(
            "Found {} buckets in the {} region out of a total of {} buckets.",
            in_region, region, num_buckets
        );
    } else {
        println!("Found {} buckets in all regions.", num_buckets);
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/s3#code-examples)\. 
+  For API details, see [ListBuckets](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 