--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Get pronunciation lexicons<a name="polly_ListLexicons_rust_topic"></a>

The following code example shows how to get Amazon Polly pronunciation lexicons\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_lexicons(client: &Client) -> Result<(), Error> {
    let resp = client.list_lexicons().send().await?;

    println!("Lexicons:");

    let lexicons = resp.lexicons.unwrap_or_default();

    for lexicon in &lexicons {
        println!(
            "  Name:     {}",
            lexicon.name.as_deref().unwrap_or_default()
        );
        println!(
            "  Language: {:?}\n",
            lexicon
                .attributes
                .as_ref()
                .map(|attrib| attrib
                    .language_code
                    .as_ref()
                    .expect("languages must have language codes"))
                .expect("languages must have attributes")
        );
    }

    println!();
    println!("Found {} lexicons.", lexicons.len());
    println!();

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/polly#code-examples)\. 
+  For API details, see [ListLexicons](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 