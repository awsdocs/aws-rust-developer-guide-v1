--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# SageMaker examples using SDK for Rust<a name="rust_sagemaker_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with SageMaker\.

*Actions* are code excerpts that show you how to call individual SageMaker functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple SageMaker functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c65c13)

## Actions<a name="w14aac14b9c65c13"></a>

### List notebook instances<a name="sagemaker_ListNotebookInstances_rust_topic"></a>

The following code example shows how to list SageMaker notebook instances\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sagemaker#code-examples)\. 
  

```
async fn show_instances(client: &Client) -> Result<(), Error> {
    let notebooks = client.list_notebook_instances().send().await?;

    println!("Notebooks:");

    for n in notebooks.notebook_instances().unwrap_or_default() {
        let n_instance_type = n.instance_type().unwrap();
        let n_status = n.notebook_instance_status().unwrap();
        let n_name = n.notebook_instance_name().unwrap_or_default();

        println!("  Name :          {}", n_name);
        println!("  Status :        {}", n_status.as_ref());
        println!("  Instance Type : {}", n_instance_type.as_ref());
        println!();
    }

    Ok(())
}
```
+  For API details, see [ListNotebookInstances](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### List training jobs<a name="sagemaker_ListTrainingJobs_rust_topic"></a>

The following code example shows how to list SageMaker training jobs\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sagemaker#code-examples)\. 
  

```
async fn show_jobs(client: &Client) -> Result<(), Error> {
    let job_details = client.list_training_jobs().send().await?;

    println!("Jobs:");

    for j in job_details.training_job_summaries().unwrap_or_default() {
        let name = j.training_job_name().unwrap_or_default();
        let creation_time = j.creation_time().unwrap().to_chrono_utc();
        let training_end_time = j.training_end_time().unwrap().to_chrono_utc();

        let status = j.training_job_status().unwrap();
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
+  For API details, see [ListTrainingJobs](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 