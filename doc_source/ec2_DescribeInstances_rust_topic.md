--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Describe instances<a name="ec2_DescribeInstances_rust_topic"></a>

The following code example shows how to describe Amazon EC2 instances\.

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

```
async fn show_state(client: &Client, ids: Vec<String>) -> Result<(), Error> {
    let resp = client
        .describe_instances()
        .set_instance_ids(Some(ids))
        .send()
        .await?;

    for reservation in resp.reservations.unwrap_or_default() {
        for instance in reservation.instances.unwrap_or_default() {
            println!("Instance ID: {}", instance.instance_id.unwrap());
            println!("State:       {:?}", instance.state.unwrap().name.unwrap());
            println!();
        }
    }

    Ok(())
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/.rust_alpha/ec2#code-examples)\. 