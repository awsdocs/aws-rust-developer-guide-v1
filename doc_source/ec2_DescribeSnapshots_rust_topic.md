--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Describe snapshots<a name="ec2_DescribeSnapshots_rust_topic"></a>

The following code example shows how to describe Amazon EBS snapshots\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
Shows the state of a snapshot\.  

```
async fn show_state(client: &Client, id: &str) -> Result<(), Error> {
    let resp = client
        .describe_snapshots()
        .filters(Filter::builder().name("snapshot-id").values(id).build())
        .send()
        .await?;

    println!(
        "State: {}",
        resp.snapshots
            .unwrap()
            .pop()
            .unwrap()
            .state
            .unwrap()
            .as_ref()
    );

    Ok(())
}
```
  

```
async fn show_snapshots(client: &Client) -> Result<(), Error> {
    // "self" represents your account ID.
    // You can list the snapshots for any account by replacing
    // "self" with that account ID.
    let resp = client.describe_snapshots().owner_ids("self").send().await?;
    let snapshots = resp.snapshots.unwrap();
    let length = snapshots.len();

    for snapshot in snapshots {
        println!(
            "ID:          {}",
            snapshot.snapshot_id.as_deref().unwrap_or_default()
        );
        println!(
            "Description: {}",
            snapshot.description.as_deref().unwrap_or_default()
        );
        println!("State:       {}", snapshot.state.unwrap().as_ref());
        println!();
    }

    println!();
    println!("Found {} snapshot(s)", length);
    println!();

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/ebs#code-examples)\. 
+  For API details, see [DescribeSnapshots](https://awslabs.github.io/aws-sdk-rust/) in *AWS SDK for Rust API reference*\. 