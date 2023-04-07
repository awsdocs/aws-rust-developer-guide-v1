--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Enforcing a minimum TLS version in the AWS SDK for Rust<a name="enforcing-tls"></a>

The AWS SDK for Rust uses TLS to increase security when communicating with AWS services\. The SDK enforces a minimum TLS version of 1\.2 by default\. By default, the SDK also negotiates the highest version of TLS available to both the client application and the service; for example, the SDK might be able to negotiate TLS 1\.3\.

A particular TLS version can be enforced in the application by providing manual configuration of the TCP connector that the SDK uses\. To illustrate this, the following example show you how to enforce TLS 1\.3\. 

**Note**  
Some AWS services do not yet support TLS 1\.3, so enforcing this version might affect SDK interoperability\. We recommend testing this configuration with each service prior to production deployment\.

```rust
pub async fn connect_via_tls_13() -> Result<(), Error> {
    println!("Attempting to connect to KMS using TLS 1.3: ");

    // Let webpki load the Mozilla root certificates.
    let mut root_store = RootCertStore::empty();
    root_store.add_server_trust_anchors(webpki_roots::TLS_SERVER_ROOTS.0.iter().map(|ta| {
        rustls::OwnedTrustAnchor::from_subject_spki_name_constraints(
            ta.subject,
            ta.spki,
            ta.name_constraints,
        )
    }));

    // The .with_protocol_versions call is where we set TLS1.3. You can add rustls::version::TLS12 or replace them both with rustls::ALL_VERSIONS
    let config = rustls::ClientConfig::builder()
        .with_safe_default_cipher_suites()
        .with_safe_default_kx_groups()
        .with_protocol_versions(&[&rustls::version::TLS13])
        .expect("It looks like your system doesn't support TLS1.3")
        .with_root_certificates(root_store)
        .with_no_client_auth();

    // Finish setup of the rustls connector.
    let rustls_connector = hyper_rustls::HttpsConnectorBuilder::new()
        .with_tls_config(config)
        .https_only()
        .enable_http1()
        .enable_http2()
        .build();

    let provider_config = ProviderConfig::default().with_tcp_connector(rustls_connector.clone());
    let shared_conf = aws_config::from_env()
        .configure(provider_config)
        .load()
        .await;

    let kms_config = aws_sdk_kms::Config::from(&shared_conf);
    let kms_client = aws_sdk_kms::Client::from_conf_conn(
        kms_config,
        hyper_ext::Adapter::builder().build(rustls_connector),
    );
    let response = kms_client.list_keys().send().await?;

    println!("{:?}", response);

    Ok(())
}
```