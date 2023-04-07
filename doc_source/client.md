--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Creating a service client in the AWS SDK for Rust<a name="client"></a>

This section describes how to create a client, including one in a specific region\.

In most cases you'll want to create a client that uses the default search path, which looks for credentials and region in the order as described in [Specifying your credentials and default region](credentials.md)\. Once it has found a value for an access key, secret key, or region, it stops searching for that value\.

You can also supply a region an argument to the client object\. Most of the [SDK for Rust code examples](rust_code_examples.md) use the following construct, which searches for the region in the search path as described previously, and if not found, sets the region to `us-east-1`, and *SERVICENAME* is the name of the service, such as `s3` for Amazon Simple Storage Service:

```rust
use aws_config::meta::region::RegionProviderChain;
use aws_sdk_SERVICENAME::Client;
    
let region_provider = RegionProviderChain::default_provider().or_else("us-east-1");
let config = aws_config::from_env().region(region_provider).load().await;
let client = Client::new(&config);
```