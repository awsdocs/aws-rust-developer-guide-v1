--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List training jobs<a name="sagemaker_ListTrainingJobs_rust_topic"></a>

The following code example shows how to list SageMaker training jobs\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_jobs(client: &Client) -> Result<(), Error> {
    let job_details = client.list_training_jobs().send().await?;

    println!("Jobs:");

    for j in job_details.training_job_summaries.unwrap_or_default() {
        let name = j.training_job_name.as_deref().unwrap_or_default();
        let creation_time = j.creation_time.unwrap().to_chrono_utc();
        let training_end_time = j.training_end_time.unwrap().to_chrono_utc();

        let status = j.training_job_status.unwrap();
        let duration = training_end_time - creation_time;

        println!("  Name:               {}", name);
        println!(
            "  Creation date/time: {}",
            creation_time.format("%Y-%m-%d@%H:%M:%S")
        );
        println!("  Duration (seconds): {}", duration.num_seconds());
        println!("  Status:             {}", status.as_ref());

        println!();
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sagemaker#code-examples)\. 
+  For API details, see [ListTrainingJobs](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 