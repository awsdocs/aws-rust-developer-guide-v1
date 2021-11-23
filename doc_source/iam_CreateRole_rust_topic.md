--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Create a role<a name="iam_CreateRole_rust_topic"></a>

The following code example shows how to create an IAM role\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn make_role(client: &Client, policy_file: &str, name: &str) -> Result<(), Error> {
    // Read policy doc from file as a string
    let doc = fs::read_to_string(policy_file).expect("Unable to read file");

    let resp = client
        .create_role()
        .assume_role_policy_document(doc)
        .role_name(name)
        .send()
        .await?;

    println!("Created role with ARN {}", resp.role.unwrap().arn.unwrap());

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/iam#code-examples)\. 
+  For API details, see [CreateRole](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 