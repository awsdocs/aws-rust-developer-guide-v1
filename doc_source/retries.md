--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Custom retries for the AWS SDK for Rust<a name="retries"></a>

This section describes how to modify the SDK to customize the number of times the SDK attempts a request before abandoning the request\.

By default the SDK attempts a request three times before abandoning the request\. You can specify the number of attempts by using an environment variable, the credentials file, or as an argument when constructing the client\. You must set the maximum number of attempts to a value greater than zero, otherwise, the SDK panics\. If you set the maximum number of attempts to 1, the SDK does not retry the request\.

## Environment variable<a name="retries_env"></a>

To set the maximum number of retries, set the `AWS_MAX_ATTEMPTS` environment variable\.

On Linux, OS X, or Unix, where *ATTEMPTS* is a value greater than zero\.

```
export AWS_MAX_ATTEMPTS=ATTEMPTS
```

On Microsoft Windows, where *ATTEMPTS* is a value greater than zero\.

```
set AWS_MAX_ATTEMPTS=ATTEMPTS
```

## In AWS profiles<a name="retries_credentials"></a>

Set the **max\_attempts** value \(`~/.aws/config` file on Linux, OS X or Unix; `%userprofile%\.aws\config` file on Microsoft Windows\) as follows, where *ATTEMPTS* is a value greater than zero\.

```
[default]
max_attempts=ATTEMPTS
```

## In code<a name="retries_code"></a>

 You can disable retries when creating the configuration for a client, as shown in the following code example\.

```
    let shared_config = aws_config::from_env()
        .region(region_provider)
        // Disable retries
        .retry_config(RetryConfig::disabled())
        .load()
        .await;
    let client = Client::new(&shared_config);
```

To override the retry configuration for a service client, specify the retry configuration when you create the service client, as shown in the following code example, where *tries* is a value greater than zero\.

```
    let shared_config = aws_config::from_env().region(region_provider).load().await;

    // Construct an S3 client with customized retry configuration.
    let client = Client::from_conf(
        // Start with the shared environment configuration.
        config::Builder::from(&shared_config)
            // Set max attempts.
            // If tries is 1, there are no retries.
            .retry_config(RetryConfig::new().with_max_attempts(tries))
            .build(),
    );
```
