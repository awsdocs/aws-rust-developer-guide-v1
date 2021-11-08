--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Creating a service client in the AWS SDK for Rust<a name="client"></a>

This section describes how to create a client, including one in a specific region\.

In most cases you'll want to create a client that uses the default search path, which looks for credentials and region in the following order\. Once it has found a value for an access key, secret key, or region, it stops searching for that value\.

1. credentials

   On Linux, OS X, or Unix, this file is found at **\~/\.aws/credentials**

   On Microsoft Windows, this file is found at **%userprofile%\\credentials**

   The SDK also looks for a region in the **config** file in the same directory\.

1. Environment variables

   See [Environment variables for the AWS SDK for Rust](environment-variables.md) for details\.

1. As an argument to the client object\. Most of the [SDK for Rust code examples](rust_code_examples.md) use the following construct, which searches for the region in the search path as described previously, and if not found, sets the region to `us-east-1`:

   ```
   use aws_config::meta::region::RegionProviderChain;
       
   let region_provider = RegionProviderChain::default_provider().or_else("us-east-1");
   let config = aws_config::from_env().region(region_provider).load().await;
   let client = Client::new(&config);
   ```