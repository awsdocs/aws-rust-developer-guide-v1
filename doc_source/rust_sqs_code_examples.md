--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Amazon SQS examples using SDK for Rust<a name="rust_sqs_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Amazon SQS\.

*Actions* are code excerpts that show you how to call individual Amazon SQS functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Amazon SQS functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c63c13)

## Actions<a name="w14aac14b9c63c13"></a>

### List queues<a name="sqs_ListQueues_rust_topic"></a>

The following code example shows how to list Amazon SQS queues\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sqs#code-examples)\. 
  

```
async fn send_receive(client: &Client) -> Result<(), Error> {
    let queues = client.list_queues().send().await?;
    let queue_urls = queues.queue_urls().unwrap_or_default();
    let queue_url = match queue_urls.first() {
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
        .queue_url(queue_url)
        .message_body("hello from my queue")
        .message_group_id("MyGroup")
        .send()
        .await?;

    println!("Response from sending a message: {:#?}", rsp);

    let rcv_message_output = client.receive_message().queue_url(queue_url).send().await?;

    for message in rcv_message_output.messages.unwrap_or_default() {
        println!("Got the message: {:#?}", message);
    }

    Ok(())
}
```
+  For API details, see [ListQueues](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Receive messages from a queue<a name="sqs_ReceiveMessage_rust_topic"></a>

The following code example shows how to receive messages from an Amazon SQS queue\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sqs#code-examples)\. 
  

```
async fn send_receive(client: &Client) -> Result<(), Error> {
    let queues = client.list_queues().send().await?;
    let queue_urls = queues.queue_urls().unwrap_or_default();
    let queue_url = match queue_urls.first() {
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
        .queue_url(queue_url)
        .message_body("hello from my queue")
        .message_group_id("MyGroup")
        .send()
        .await?;

    println!("Response from sending a message: {:#?}", rsp);

    let rcv_message_output = client.receive_message().queue_url(queue_url).send().await?;

    for message in rcv_message_output.messages.unwrap_or_default() {
        println!("Got the message: {:#?}", message);
    }

    Ok(())
}
```
+  For API details, see [ReceiveMessage](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Send a message to a queue<a name="sqs_SendMessage_rust_topic"></a>

The following code example shows how to send a message to an Amazon SQS queue\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sqs#code-examples)\. 
  

```
async fn send_receive(client: &Client) -> Result<(), Error> {
    let queues = client.list_queues().send().await?;
    let queue_urls = queues.queue_urls().unwrap_or_default();
    let queue_url = match queue_urls.first() {
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
        .queue_url(queue_url)
        .message_body("hello from my queue")
        .message_group_id("MyGroup")
        .send()
        .await?;

    println!("Response from sending a message: {:#?}", rsp);

    let rcv_message_output = client.receive_message().queue_url(queue_url).send().await?;

    for message in rcv_message_output.messages.unwrap_or_default() {
        println!("Got the message: {:#?}", message);
    }

    Ok(())
}
```
+  For API details, see [SendMessage](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 