--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Detect faces in an image using an AWS SDK<a name="cross_DetectFaces_rust_topic"></a>

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 Save the image in an Amazon S3 bucket with an **uploads** prefix, use Amazon Rekognition to detect facial details, such as age range, gender, and emotion \(smiling, etc\.\), and display those details\.   
 For complete source code and instructions on how to set up and run, see the full example on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/rust_dev_preview/cross_service/detect_faces/src/main.rs)\.   

**Services used in this example**
+ Amazon Rekognition
+ Amazon S3