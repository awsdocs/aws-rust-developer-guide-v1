--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Amazon SNS examples using SDK for Rust<a name="rust_sns_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Amazon SNS\.

*Actions* are code excerpts that show you how to call individual Amazon SNS functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Amazon SNS functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c61c13)

## Actions<a name="w14aac14b9c61c13"></a>

### Create a topic<a name="sns_CreateTopic_rust_topic"></a>

The following code example shows how to create an Amazon SNS topic\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sns#code-examples)\. 
  

```rust
async fn make_topic(client: &Client, topic_name: &str) -> Result<(), Error> {
    let resp = client.create_topic().name(topic_name).send().await?;

    println!(
        "Created topic with ARN: {}",
        resp.topic_arn().unwrap_or_default()
    );

    Ok(())
}
```
+  For API details, see [CreateTopic](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### List topics<a name="sns_ListTopics_rust_topic"></a>

The following code example shows how to list Amazon SNS topics\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sns#code-examples)\. 
  

```rust
async fn show_topics(client: &Client) -> Result<(), Error> {
    let resp = client.list_topics().send().await?;

    println!("Topic ARNs:");

    for topic in resp.topics().unwrap_or_default() {
        println!("{}", topic.topic_arn().unwrap_or_default());
    }

    Ok(())
}
```
+  For API details, see [ListTopics](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Publish to a topic<a name="sns_Publish_rust_topic"></a>

The following code example shows how to publish messages to an Amazon SNS topic\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sns#code-examples)\. 
  

```rust
async fn subscribe_and_publish(
    client: &Client,
    topic_arn: &str,
    email_address: &str,
) -> Result<(), Error> {
    println!("Receiving on topic with ARN: `{}`", topic_arn);

    let rsp = client
        .subscribe()
        .topic_arn(topic_arn)
        .protocol("email")
        .endpoint(email_address)
        .send()
        .await?;

    println!("Added a subscription: {:?}", rsp);

    let rsp = client
        .publish()
        .topic_arn(topic_arn)
        .message("hello sns!")
        .send()
        .await?;

    println!("Published message: {:?}", rsp);

    Ok(())
}
```
+  For API details, see [Publish](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Subscribe an email address to a topic<a name="sns_Subscribe_rust_topic"></a>

The following code example shows how to subscribe an email address to an Amazon SNS topic\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sns#code-examples)\. 
  

```rust
async fn subscribe_and_publish(
    client: &Client,
    topic_arn: &str,
    email_address: &str,
) -> Result<(), Error> {
    println!("Receiving on topic with ARN: `{}`", topic_arn);

    let rsp = client
        .subscribe()
        .topic_arn(topic_arn)
        .protocol("email")
        .endpoint(email_address)
        .send()
        .await?;

    println!("Added a subscription: {:?}", rsp);

    let rsp = client
        .publish()
        .topic_arn(topic_arn)
        .message("hello sns!")
        .send()
        .await?;

    println!("Published message: {:?}", rsp);

    Ok(())
}
```
+  For API details, see [Subscribe](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 