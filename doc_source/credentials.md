--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Specifying your credentials and default region<a name="credentials"></a>

To make an API call using the AWS SDK for Rust \(the SDK\), you must supply credentials\. Two values comprise credentials: an Access Key ID and a Secret Access Key\. If you haven't created these two values, see the [Getting started with the AWS SDK for Rust](getting-started.md) topic for information on retrieving your credentials\.

Once you have your credentials, the SDK can access them from any of the following locations \(when it finds credentials, the SDK quits looking further\):
+ In the environment variables **AWS\_ACCESS\_KEY\_ID** and **AWS\_SECRET\_ACCESS\_KEY**\.
+ In [Web Identity Token credentials](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html) from the environment or container \(including EKS\)
+  [ECS credentials \(IAM roles for tasks\)](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html) 
+ As entries in the **credentials** file in the **\.aws** directory in your home directory \(**\~/\.aws/credentials** on Linux, OS X, and Unix; **%userprofile%\\\.aws\\credentials** on Microsoft Windows\):

  ```
  [default]
  aws_access_key_id=YOUR-ACCESS-KEY
  aws_secret_access_key=YOUR-SECRET-KEY
  ```
+ From the [EC2 Instance Metadata Service](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) \(IAM Roles attached to an instance\)

## Specifying a region<a name="credentials-region"></a>

Since most resources live in a specific AWS Region, it's imperative that you supply the correct region for the resource when using the SDK\. Much as the SDK searches for credentials in a specific order, so does it search for a region\. The SDK looks in the following order for a default region:
+ In the environment variable **AWS\_REGION**\.
+ As an entry in the **credentials** file in the **\.aws** directory in your home directory \(**\~/\.aws/credentials** on Linux, OS X, and Unix; **%userprofile%\\\.aws\\credentials** on Microsoft Windows\):

  ```
  [default]
  region=YOUR-DEFAULT-REGION
  ```
+ As an entry in the **config** file in the **\.aws** directory in your home directory \(**\~/\.aws/config** on Linux, OS X, and Unix; **%userprofile%\\\.aws\\config** on Microsoft Windows\):

  ```
  [default]
  region=YOUR-DEFAULT-REGION
  ```
