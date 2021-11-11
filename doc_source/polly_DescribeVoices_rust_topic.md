--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Get the voices available for synthesis<a name="polly_DescribeVoices_rust_topic"></a>

The following code example shows how to get the Amazon Polly voices available for synthesis\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn list_voices(client: &Client) -> Result<(), Error> {
    let resp = client.describe_voices().send().await?;

    println!("Voices:");

    let voices = resp.voices.unwrap_or_default();
    for voice in &voices {
        println!(
            "  Name:     {}",
            voice.name.as_deref().unwrap_or("No name!")
        );
        println!(
            "  Language: {}",
            voice.language_name.as_deref().unwrap_or("No language!")
        );

        println!();
    }

    println!("Found {} voices", voices.len());

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/polly#code-examples)\. 
+  For API details, see [DescribeVoices](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 