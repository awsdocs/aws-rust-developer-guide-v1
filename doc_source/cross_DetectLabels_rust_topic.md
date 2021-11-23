--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Save EXIF and other image information using an AWS SDK<a name="cross_DetectLabels_rust_topic"></a>

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 Get EXIF information from a JPG, JPEG, or PNG file, upload the image file to an Amazon Simple Storage Service bucket, use Amazon Rekognition to identify the three top attributes \(*labels* in Amazon Rekognition\) in the file, and add the EXIF and label information to a Amazon DynamoDB table in the Region\.   
 For complete source code and instructions on how to set up and run, see the full example on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/rust_dev_preview/cross_service/detect_labels/src/main.rs)\.   

**Services used in this example**
+ DynamoDB
+ Amazon Rekognition
+ Amazon S3