--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# S3 Object Lambda endpoint for the AWS SDK for Rust<a name="s3-object-lambda"></a>

With [S3 Object Lambda](https://docs.aws.amazon.com/AmazonS3/latest/userguide/transforming-objects.html) you can add your own code to Amazon S3 GET requests to modify and process data as it is returned to an application\.

The AWS SDK for Rust \(SDK\) does not have built\-in support for Amazon S3 Object Lambda\. To work around this, you can configure a custom endpoint resolver to use the correct Object Lambda endpoint\.

1. Add a dependency on `aws-endpoint` to `Cargo.toml`, where *VERSION* is the latest version of `aws-endpoint` on [crates\.io](https://crates.io/search?q=aws-endpoint):

   ```
   aws-endpoint = { git = "https://github.com/awslabs/aws-sdk-rust", branch = "next" }
   ```

1. Override the endpoint resolver so that it uses the S3 Object Lambda endpoint by hard\-coding the access point name and AWS account ID number into a URI template, where *$accountNumber* is your account number and *$accessPoint* is the name of your S3 Object Lambda access point:

   ```
   use aws_endpoint::partition;
   use aws_endpoint::partition::endpoint;
   use aws_endpoint::{CredentialScope, Partition, PartitionResolver};
   use aws_sdk_s3 as s3;
   ```

   ```
       // Create an endpoint resolver that creates S3 Object Lambda endpoints.
       let resolver = PartitionResolver::new(
           Partition::builder()
               .id("aws")
               // This regex captures the region prefix, such as the "us" in "us-east-1",
               //  from the client's current region. This captured value is later fed into
               //  the uri_template.
               // If your region isn't covered by the regex below,
               // you can find additional region capture regexes for other regions
               // at https://github.com/awslabs/aws-sdk-rust/blob/main/sdk/s3/src/aws_endpoint.rs.
               .region_regex(r#"^(us|eu|ap|sa|ca|me|af)\-\w+\-\d+$"#)
               .default_endpoint(endpoint::Metadata {
                   uri_template: make_uri(&endpoint, &account).await,
                   protocol: endpoint::Protocol::Https,
                   signature_versions: endpoint::SignatureVersion::V4,
                   // Important: The following overrides the credential scope so that request signing works.
                   credential_scope: CredentialScope::builder()
                       .service("s3-object-lambda")
                       .build(),
               })
               .regionalized(partition::Regionalized::Regionalized)
               .build()
               .expect("valid partition"),
           vec![],
       );
   
       // Load configuration and credentials from the environment.
       let shared_config = aws_config::load_from_env().await;
   
       // Create an S3 config from the shared config and override the endpoint resolver.
       let s3_config = s3::config::Builder::from(&shared_config)
           .endpoint_resolver(resolver)
           .build();
   
       // Create an S3 client to send requests to S3 Object Lambda.
       let client = s3::Client::from_conf(s3_config);
   ```