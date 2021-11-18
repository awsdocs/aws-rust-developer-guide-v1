--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# List topics<a name="sns_ListTopics_rust_topic"></a>

The following code example shows how to list Amazon SNS topics\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_topics(client: &Client) -> Result<(), Error> {
    let resp = client.list_topics().send().await?;

    println!("Topic ARNs:");

    for topic in resp.topics.unwrap_or_default() {
        println!("{}", topic.topic_arn.as_deref().unwrap_or_default());
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/sns#code-examples)\. 
+  For API details, see [ListTopics](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 