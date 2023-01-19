--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Amazon ECS examples using SDK for Rust<a name="rust_ecs_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Amazon ECS\.

*Actions* are code excerpts that show you how to call individual Amazon ECS functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Amazon ECS functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c33c13)

## Actions<a name="w14aac14b9c33c13"></a>

### Create a cluster<a name="ecs_CreateCluster_rust_topic"></a>

The following code example shows how to create an Amazon ECS cluster\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ecs#code-examples)\. 
  

```
async fn make_cluster(client: &aws_sdk_ecs::Client, name: &str) -> Result<(), aws_sdk_ecs::Error> {
    let cluster = client.create_cluster().cluster_name(name).send().await?;
    println!("cluster created: {:?}", cluster);

    Ok(())
}
```
+  For API details, see [CreateCluster](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Delete a cluster<a name="ecs_DeleteCluster_rust_topic"></a>

The following code example shows how to delete an Amazon ECS cluster\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ecs#code-examples)\. 
  

```
async fn remove_cluster(
    client: &aws_sdk_ecs::Client,
    name: &str,
) -> Result<(), aws_sdk_ecs::Error> {
    let cluster_deleted = client.delete_cluster().cluster(name).send().await?;
    println!("cluster deleted: {:?}", cluster_deleted);

    Ok(())
}
```
+  For API details, see [DeleteCluster](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### List your clusters<a name="ecs_DescribeClusters_rust_topic"></a>

The following code example shows how to list your Amazon ECS clusters\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ecs#code-examples)\. 
  

```
async fn show_clusters(client: &aws_sdk_ecs::Client) -> Result<(), aws_sdk_ecs::Error> {
    let resp = client.describe_clusters().send().await?;

    let clusters = resp.clusters().unwrap_or_default();
    println!("Found {} clusters:", clusters.len());

    for cluster in clusters {
        println!("  ARN:  {}", cluster.cluster_arn().unwrap());
        println!("  Name: {}", cluster.cluster_name().unwrap());
    }
    Ok(())
}
```
+  For API details, see [DescribeClusters](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 