--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# MediaLive examples using SDK for Rust<a name="rust_medialive_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with MediaLive\.

*Actions* are code excerpts that show you how to call individual MediaLive functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple MediaLive functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c45c13)

## Actions<a name="w14aac14b9c45c13"></a>

### List inputs<a name="medialive_ListInputs_rust_topic"></a>

The following code example shows how to list your MediaLive inputs\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
List your MediaLive input names and ARNs in the Region\.  

```
async fn show_inputs(client: &Client) -> Result<(), Error> {
    let input_list = client.list_inputs().send().await?;

    for i in input_list.inputs().unwrap_or_default() {
        let input_arn = i.arn().unwrap_or_default();
        let input_name = i.name().unwrap_or_default();

        println!("Input Name : {}", input_name);
        println!("Input ARN : {}", input_arn);
        println!();
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/medialive#code-examples)\. 
+  For API details, see [ListInputs](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 