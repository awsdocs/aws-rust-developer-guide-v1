--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Route 53 examples using SDK for Rust<a name="rust_route-53_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Route 53\.

*Actions* are code excerpts that show you how to call individual Route 53 functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Route 53 functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c55c13)

## Actions<a name="w14aac14b9c55c13"></a>

### Get a list of the public and private hosted zones<a name="route-53_ListHostedZones_rust_topic"></a>

The following code example shows how to get a list of the public and private hosted zones\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_host_info(client: &aws_sdk_route53::Client) -> Result<(), aws_sdk_route53::Error> {
    let hosted_zone_count = client.get_hosted_zone_count().send().await?;

    println!(
        "Number of hosted zones in region : {}",
        hosted_zone_count.hosted_zone_count().unwrap_or_default(),
    );

    let hosted_zones = client.list_hosted_zones().send().await?;

    println!("Zones:");

    for hz in hosted_zones.hosted_zones().unwrap_or_default() {
        let zone_name = hz.name().unwrap_or_default();
        let zone_id = hz.id().unwrap_or_default();

        println!("  ID :   {}", zone_id);
        println!("  Name : {}", zone_name);
        println!();
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/route53#code-examples)\. 
+  For API details, see [ListHostedZones](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 