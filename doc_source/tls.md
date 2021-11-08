--------

 *This is documentation for a prerelease of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Using native TLS in the AWS SDK for Rust<a name="tls"></a>

This section describes how to override the default TLS handler in the SDK with native TLS\.

The standard entry in **Cargo\.toml** for the **aws\-config** crate looks something like the following, where *VERSION* is the specific version:

```
aws-config = "VERSION"
```

To use native TLS, you first have to disable the default features, which includes **rustls**\. Then you must re\-enable the default features you still need, and include native TLS:

```
{ version = "VERSION", no-default-features = true, features = ["default-provider", "native-tls", "rt-tokio", "dns", "tcp-connector"] }
```