--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Amazon EC2 examples using SDK for Rust<a name="rust_ec2_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for Rust with Amazon EC2\.

*Actions* are code excerpts that show you how to call individual Amazon EC2 functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Amazon EC2 functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w14aac14b9c27c13)

## Actions<a name="w14aac14b9c27c13"></a>

### Delete a snapshot<a name="ec2_DeleteSnapshot_rust_topic"></a>

The following code example shows how to delete an Amazon EBS snapshot\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ebs#code-examples)\. 
  

```rust
async fn delete_snapshot(client: &Client, id: &str) -> Result<(), Error> {
    client.delete_snapshot().snapshot_id(id).send().await?;

    println!("Deleted");

    Ok(())
}
```
+  For API details, see [DeleteSnapshot](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Describe Regions<a name="ec2_DescribeRegions_rust_topic"></a>

The following code example shows how to describe Amazon EC2 Regions\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ec2#code-examples)\. 
  

```rust
async fn show_regions(client: &Client) -> Result<(), Error> {
    let rsp = client.describe_regions().send().await?;

    println!("Regions:");
    for region in rsp.regions().unwrap_or_default() {
        println!("  {}", region.region_name().unwrap());
    }

    Ok(())
}
```
+  For API details, see [DescribeRegions](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Describe instance status<a name="ec2_DescribeInstanceStatus_rust_topic"></a>

The following code example shows how to describe the status of Amazon EC2 instances\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ec2#code-examples)\. 
  

```rust
async fn show_all_events(client: &Client) -> Result<(), Error> {
    let resp = client.describe_regions().send().await.unwrap();

    for region in resp.regions.unwrap_or_default() {
        let reg: &'static str = Box::leak(Box::from(region.region_name().unwrap()));
        let region_provider = RegionProviderChain::default_provider().or_else(reg);
        let config = aws_config::from_env().region(region_provider).load().await;
        let new_client = Client::new(&config);

        let resp = new_client.describe_instance_status().send().await;

        println!("Instances in region {}:", reg);
        println!();

        for status in resp.unwrap().instance_statuses().unwrap_or_default() {
            println!(
                "  Events scheduled for instance ID: {}",
                status.instance_id().as_deref().unwrap_or_default()
            );
            for event in status.events().unwrap_or_default() {
                println!("    Event ID:     {}", event.instance_event_id().unwrap());
                println!("    Description:  {}", event.description().unwrap());
                println!("    Event code:   {}", event.code().unwrap().as_ref());
                println!();
            }
        }
    }

    Ok(())
}
```
+  For API details, see [DescribeInstanceStatus](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Describe instances<a name="ec2_DescribeInstances_rust_topic"></a>

The following code example shows how to describe Amazon EC2 instances\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ec2#code-examples)\. 
  

```rust
async fn show_state(client: &Client, ids: Option<Vec<String>>) -> Result<(), Error> {
    let resp = client
        .describe_instances()
        .set_instance_ids(ids)
        .send()
        .await?;

    for reservation in resp.reservations().unwrap_or_default() {
        for instance in reservation.instances().unwrap_or_default() {
            println!("Instance ID: {}", instance.instance_id().unwrap());
            println!(
                "State:       {:?}",
                instance.state().unwrap().name().unwrap()
            );
            println!();
        }
    }

    Ok(())
}
```
+  For API details, see [DescribeInstances](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Describe snapshots<a name="ec2_DescribeSnapshots_rust_topic"></a>

The following code example shows how to describe Amazon EBS snapshots\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ebs#code-examples)\. 
Shows the state of a snapshot\.  

```rust
async fn show_state(client: &Client, id: &str) -> Result<(), Error> {
    let resp = client
        .describe_snapshots()
        .filters(Filter::builder().name("snapshot-id").values(id).build())
        .send()
        .await?;

    println!(
        "State: {}",
        resp.snapshots()
            .unwrap()
            .first()
            .unwrap()
            .state()
            .unwrap()
            .as_ref()
    );

    Ok(())
}
```
  

```rust
async fn show_snapshots(client: &Client) -> Result<(), Error> {
    // "self" represents your account ID.
    // You can list the snapshots for any account by replacing
    // "self" with that account ID.
    let resp = client.describe_snapshots().owner_ids("self").send().await?;
    let snapshots = resp.snapshots().unwrap();
    let length = snapshots.len();

    for snapshot in snapshots {
        println!(
            "ID:          {}",
            snapshot.snapshot_id().as_deref().unwrap_or_default()
        );
        println!(
            "Description: {}",
            snapshot.description().as_deref().unwrap_or_default()
        );
        println!("State:       {}", snapshot.state().unwrap().as_ref());
        println!();
    }

    println!();
    println!("Found {} snapshot(s)", length);
    println!();

    Ok(())
}
```
+  For API details, see [DescribeSnapshots](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Enable monitoring<a name="ec2_MonitorInstances_rust_topic"></a>

The following code example shows how to enable monitoring for a running Amazon EC2 instance\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ec2#code-examples)\. 
  

```rust
async fn enable_monitoring(client: &Client, id: &str) -> Result<(), Error> {
    client.monitor_instances().instance_ids(id).send().await?;

    println!("Enabled monitoring");

    Ok(())
}
```
+  For API details, see [MonitorInstances](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Reboot an instance<a name="ec2_RebootInstances_rust_topic"></a>

The following code example shows how to reboot an Amazon EC2 instance\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ec2#code-examples)\. 
  

```rust
async fn reboot_instance(client: &Client, id: &str) -> Result<(), Error> {
    client.reboot_instances().instance_ids(id).send().await?;

    println!("Rebooted instance.");
    Ok(())
}
```
+  For API details, see [RebootInstances](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Start an instance<a name="ec2_StartInstances_rust_topic"></a>

The following code example shows how to start an Amazon EC2 instance\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ec2#code-examples)\. 
  

```rust
async fn start_instance(client: &Client, id: &str) -> Result<(), Error> {
    client.start_instances().instance_ids(id).send().await?;

    println!("Started instance.");

    Ok(())
}
```
+  For API details, see [StartInstances](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

### Stop an instance<a name="ec2_StopInstances_rust_topic"></a>

The following code example shows how to stop an Amazon EC2 instance\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/ec2#code-examples)\. 
  

```rust
async fn stop_instance(client: &Client, id: &str) -> Result<(), Error> {
    client.stop_instances().instance_ids(id).send().await?;

    println!("Stopped instance.");

    Ok(())
}
```
+  For API details, see [StopInstances](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 