--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Amazon EKS examples using SDK for Rust<a name="rust_eks_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Amazon EKS\.

*Actions* are code excerpts that show you how to call individual Amazon EKS functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Amazon EKS functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c35c13)

## Actions<a name="w14aac14b9c35c13"></a>

### Create a cluster control plane<a name="eks_CreateCluster_rust_topic"></a>

The following code example shows how to create an Amazon EKS cluster control plane\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/eks#code-examples)\. 
  

```
async fn make_cluster(
    client: &aws_sdk_eks::Client,
    name: &str,
    arn: &str,
    subnet_ids: Vec<String>,
) -> Result<(), aws_sdk_eks::Error> {
    let cluster = client
        .create_cluster()
        .name(name)
        .role_arn(arn)
        .resources_vpc_config(
            VpcConfigRequest::builder()
                .set_subnet_ids(Some(subnet_ids))
                .build(),
        )
        .send()
        .await?;
    println!("cluster created: {:?}", cluster);

    Ok(())
}
```
+  For API details, see [CreateCluster](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Delete a cluster control plane<a name="eks_DeleteCluster_rust_topic"></a>

The following code example shows how to delete an Amazon EKS cluster\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/eks#code-examples)\. 
  

```
async fn remove_cluster(
    client: &aws_sdk_eks::Client,
    name: &str,
) -> Result<(), aws_sdk_eks::Error> {
    let cluster_deleted = client.delete_cluster().name(name).send().await?;
    println!("cluster deleted: {:?}", cluster_deleted);

    Ok(())
}
```
+  For API details, see [DeleteCluster](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 