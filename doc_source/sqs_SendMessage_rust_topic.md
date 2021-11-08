--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Send a message to a queue<a name="sqs_SendMessage_rust_topic"></a>

The following code example shows how to send a message to an Amazon SQS queue\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn send_receive(client: &Client) -> Result<(), Error> {
    let queues = client.list_queues().send().await?;
    let mut queue_urls = queues.queue_urls.unwrap_or_default();
    let queue_url = match queue_urls.pop() {
        Some(url) => url,
        None => {
            eprintln!("No queues in this account. Please create a queue to proceed");
            exit(1);
        }
    };

    println!(
        "Sending and receiving messages on with URL: `{}`",
        queue_url
    );

    let rsp = client
        .send_message()
        .queue_url(&queue_url)
        .message_body("hello from my queue")
        .message_group_id("MyGroup")
        .send()
        .await?;

    println!("Response from sending a message: {:#?}", rsp);

    let rcv_message_output = client
        .receive_message()
        .queue_url(&queue_url)
        .send()
        .await?;

    for message in rcv_message_output.messages.unwrap_or_default() {
        println!("Got the message: {:#?}", message);
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/sqs#code-examples)\. 
+  For API details, see [SendMessage](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 