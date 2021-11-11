--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List notebook instances<a name="sagemaker_ListNotebookInstances_rust_topic"></a>

The following code example shows how to list SageMaker notebook instances\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_instances(client: &Client) -> Result<(), Error> {
    let notebooks = client.list_notebook_instances().send().await?;

    println!("Notebooks:");

    for n in notebooks.notebook_instances.unwrap_or_default() {
        let n_instance_type = n.instance_type.unwrap();
        let n_status = n.notebook_instance_status.unwrap();
        let n_name = n.notebook_instance_name.as_deref().unwrap_or_default();

        println!("  Name :          {}", n_name);
        println!("  Status :        {}", n_status.as_ref());
        println!("  Instance Type : {}", n_instance_type.as_ref());
        println!();
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/sagemaker#code-examples)\. 
+  For API details, see [ListNotebookInstances](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 