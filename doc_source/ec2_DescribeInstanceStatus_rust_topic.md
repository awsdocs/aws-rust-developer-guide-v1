--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Describe status<a name="ec2_DescribeInstanceStatus_rust_topic"></a>

The following code example shows how to describe Amazon EC2 status\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_all_events(client: &Client) -> Result<(), Error> {
    let resp = client.describe_regions().send().await.unwrap();

    for region in resp.regions.unwrap_or_default() {
        let reg: &'static str = Box::leak(region.region_name.unwrap().into_boxed_str());
        let region_provider = RegionProviderChain::default_provider().or_else(reg);
        let config = aws_config::from_env().region(region_provider).load().await;
        let new_client = Client::new(&config);

        let resp = new_client.describe_instance_status().send().await;

        println!("Instances in region {}:", reg);
        println!();

        for status in resp.unwrap().instance_statuses.unwrap_or_default() {
            println!(
                "  Events scheduled for instance ID: {}",
                status.instance_id.as_deref().unwrap_or_default()
            );
            for event in status.events.unwrap_or_default() {
                println!("    Event ID:     {}", event.instance_event_id.unwrap());
                println!("    Description:  {}", event.description.unwrap());
                println!("    Event code:   {}", event.code.unwrap().as_ref());
                println!();
            }
        }
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/ec2#code-examples)\. 