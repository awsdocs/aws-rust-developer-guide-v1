--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Environment variables for the AWS SDK for Rust<a name="environment-variables"></a>

This section describes the environment variables that the AWS SDK for Rust recognizes\.

## Setting environment variables<a name="environment-variables-setting"></a>

To set the environment variable *MyVar* to *MyValue* Linux, OS X, or Unix:

```
export MyVar=MyValue
```

To set the environment variable *MyVar* to *MyValue* Microsoft Windows:

```
set MyVar=MyValue
```

**Warning**  
Be careful that you don't accidently include a space in an environment variable, especially when creating a default region\. For example, if you set:  

```
export AWS_REGION=" us-west-2"
```
And call an SDK function, you get an error message that there was an invalid character:  

```
 Unhandled(ConstructionFailure(EndpointResolutionError(InvalidUri(InvalidUriChar))))
```

The remainder of this topic describes the environment variables by type\.

## Environment variables for credentials<a name="environment-variables-credentials"></a>

The SDK recognizes the following environment variables related to credentials\.

### Environment variables for basic credentials<a name="environment-variables-credentials-basic"></a>

The SDK recognizes the following environment variables related to basic credentials\. See [Specifying your credentials and default region](credentials.md)

**AWS\_ACCESS\_KEY\_ID**  
Your access key ID  
When set, this is the highest priorty credential\.

**AWS\_SECRET\_ACCESS\_KEY**  
Your secret access key\.

**AWS\_SESSION\_TOKEN**  
A session token  
Used when credentials include a session token\.

**SECRET\_ACCESS\_KEY**  
Alternative variable for secret access key\.  
For compatibility only, use `AWS_SECRET_ACCESS_KEY` instead\.

### Environment variables for ECS/HTTP credentials<a name="environment-variables-credentials-ecs"></a>

The SDK recognizes the following environment variables related to ECS/HTTP credentials\.

**AWS\_CONTAINER\_AUTHORIZATION\_TOKEN**  
Authorization token for ECS/HTTP provider\.  
The value is sent in the AUTHORIZATION header, so it must be a valid header value\.

**AWS\_CONTAINER\_CREDENTIALS\_FULL\_URI**  
Set a full URI \(including authority\) for container credentials\. Cannot be set with `AWS_CONTAINER_CREDENTIALS_RELATIVE_URI`\. Must use HTTPS or resolve to a loopback address\.  
Set this environment variable to support your own HTTP credentials loader\.

**AWS\_CONTAINER\_CREDENTIALS\_RELATIVE\_URI**  
Relative URI for the ECS/HTTP credentials provider\. Relative to `http://169.254.170.2`\.  
This is set by ECS, and rarely set by users\.

### Environment variables for web identity token/EKS credentials<a name="environment-variables-credentials-web"></a>

The SDK recognizes the following environment variables related to web identity token/EKS credentials\.

**AWS\_ROLE\_ARN**  
The ARN of the role to assume using the web identity token\.

**AWS\_ROLE\_SESSION\_NAME**  
The session name to use during assume role\.  
Optional\. If not provided, the SDK will generate a default value\.

**AWS\_WEB\_IDENTITY\_TOKEN\_FILE**  
The token file use by the web identity token provider\.  
This is set by ECS, and rarely set by users\.

### IMDS credentials<a name="environment-variables-credentials-imds"></a>

The SDK recognizes the following environment variables related to EC2 Instance Metadata Service \(IMDS\) credentials\.

**AWS\_EC2\_METADATA\_DISABLED**  
Disables IMDS\.  
Profile key: none  
If set, the IMDS credential and region provider are not used as part of the default credentials chain\.

**AWS\_EC2\_METADATA\_SERVICE\_ENDPOINT**  
Overrides the endpoint used for IMDS\.  
Profile key: `ec2_metadata_service_endpoint`  
For example: `http://myimds`

**AWS\_EC2\_METADATA\_SERVICE\_ENDPOINT\_MODE**  
Sets the mode used for IDMS: **IPv4** or **IPv6**\.  
Profile key: `ec2_metadata_service_endpoint_mode`  
If `AWS_EC2_METADATA_SERVICE_ENDPOINT` is set, this variable is ignored\.

## Environment variables for your profile<a name="environment-variables-profile"></a>

The SDK recognizes the following environment variables related to your profile\.

AWS\_CONFIG\_FILE  
Overrides the default location of the `config` file\.  
Default location: `~/aws/config` on Linux, OS X, or Unix; `%userprofile%\.aws\config` on Microsoft Windows\.

AWS\_PROFILE  
Overrides the name of the profile to use for all configuration settings\.  
Default name: **default**\.

AWS\_SHARED\_CREDENTIALS\_FILE  
Overrides the location of the `credentials` file\.  
Default location: Default location: `~/aws/credentials` on Linux, OS X, or Unix; `%userprofile%\.aws\credentials` on Microsoft Windows\.

### Home directory resolution<a name="environment-variables-profile-home"></a>

If the config file contains a tilde \(**\~**\), the SDK attempts home directory resolution\. Home directory resolution is used when:
+ The tilde is the first character in the path\.
+ It is immediately followed by a slash \(/ on Linux, OS X, or Unix\) or backslash \(\\ on Microsoft Windows\)\.

When determining the home directory, the following environment variables are checked:
+ `HOME` on all platforms\.
+ `USERPROFILE` on Microsoft Windows\.
+ The concatenation of `HOMEDRIVE` and `HOMEPATH` on Microsoft Windows \(`$HOMEDRIVE$HOMEPATH`\.\)

## Environment variables for AWS Region<a name="environment-variables-region"></a>

The SDK recognizes the following environment variables related to the Region\.

AWS\_REGION  
The Region to use when making and signing requests\.  
For example: `us-east-1`\. See [Specifying a region](credentials.md#credentials-region)

## Miscellaneous environment variables<a name="environment-variables-misc"></a>

The SDK recognizes the following additional environment variables\.

AWS\_MAX\_ATTEMPTS  
The maximum number of total attempts to use when dispatching a request\.  
Setting this value 0 is invalid\. To disable retries, set this value to 1\. The default is 3\. See [Custom retries](retries.md)\.

AWS\_RETRY\_MODE  
The retry mode to use\.  
The only valid value is **STANDARD**\.