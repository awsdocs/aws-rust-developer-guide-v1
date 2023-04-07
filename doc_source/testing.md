--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Testing your code<a name="testing"></a>

This section describes a couple of approaches to unit testing code written with the SDK\.
+ Create an object trait to abstract the SDK calls away so that they can be faked during unit tests\.
+ Create an enum to abstract the SDK calls away, similar to the previous approach\.

Let’s look at a concrete example to illustrate\. The following example calls Amazon S3 over and over again with pagination in order to get the complete file size of a prefix in the bucket:

```rust
// So we can refer to the S3 package as s3 for the rest of the example.
use aws_sdk_s3 as s3;
```

```rust
// Lists all objects in an S3 bucket with the given prefix, and adds up their size.
async fn determine_prefix_file_size(
    s3: s3::Client,
    bucket: &str,
    prefix: &str,
) -> Result<usize, Box<dyn Error + Send + Sync + 'static>> {
    let mut next_token: Option<String> = None;
    let mut total_size_bytes = 0;
    loop {
        let response = s3
            .list_objects_v2()
            .bucket(bucket)
            .prefix(prefix)
            .set_continuation_token(next_token.take())
            .send()
            .await?;

        // Add up the file sizes we got back
        if let Some(contents) = response.contents() {
            for object in contents {
                total_size_bytes += object.size() as usize;
            }
        }

        // Handle pagination, and break the loop if there are no more pages
        next_token = response.continuation_token().map(|t| t.to_string());
        if !response.is_truncated() {
            break;
        }
    }
    Ok(total_size_bytes)
}
```

This code is sophisticated enough that a unit test is warranted, and the SDK is tightly coupled with it, which makes testing it difficult\.

Let's look at the different approaches that we can use to unit test this code\.

## Trait objects<a name="testing-1"></a>

This approach uses trait objects to unit test your code\.

For this approach, we take advantage of Rust’s trait objects feature\. If you’re unfamiliar, the Rust Book goes into great detail on [trait objects](https://doc.rust-lang.org/book/ch17-02-trait-objects.html) and how to use them\. Note that since Rust doesn’t currently support async functions in traits, we use the [async\-trait](https://crates.io/crates/async-trait) crate to enable async functions in traits\.

1. Add the following entry to the `Cargo.toml` file\.

   ```toml
   [dependencies]
   async-trait = "0.1.51"
   ```

1. Next, lets define our trait\. Since we want to test the pagination and summation logic in our function, it makes sense to abstract out the call to `ListObjectsV2` from Amazon S3, so we’ll make a trait for that\.

   ```rust
   pub struct ListObjectsResult {
       pub objects: Vec<s3::model::Object>,
       pub continuation_token: Option<String>,
       pub has_more: bool,
   }
   
   #[async_trait]
   pub trait ListObjects {
       async fn list_objects(
           &self,
           bucket: &str,
           prefix: &str,
           continuation_token: Option<String>,
       ) -> Result<ListObjectsResult, Box<dyn Error + Send + Sync + 'static>>;
   }
   ```

1. Now we can refactor the function to use this trait instead of the actual Amazon S3 client:

   ```rust
   async fn determine_prefix_file_size(
       // Now we take a reference to our trait object instead of the S3 client
       list_objects_impl: &dyn ListObjects,
       bucket: &str,
       prefix: &str,
   ) -> Result<usize, Box<dyn Error + Send + Sync + 'static>> {
       let mut next_token: Option<String> = None;
       let mut total_size_bytes = 0;
       loop {
           let result = list_objects_impl
               .list_objects(bucket, prefix, next_token.take())
               .await?;
   
           // Add up the file sizes we got back
           for object in result.objects {
               total_size_bytes += object.size() as usize;
           }
   
           // Handle pagination, and break the loop if there are no more pages
           next_token = result.continuation_token;
           if !result.has_more {
               break;
           }
       }
       Ok(total_size_bytes)
   }
   ```

1. Now that our implementation is successfully abstracted, it can be unit tested\. Add the actual Amazon S3 implementation:

   ```rust
   #[derive(Clone, Debug)]
   pub struct S3ListObjects {
       s3: s3::Client,
   }
   
   impl S3ListObjects {
       pub fn new(s3: s3::Client) -> Self {
           Self { s3 }
       }
   }
   
   #[async_trait]
   impl ListObjects for S3ListObjects {
       async fn list_objects(
           &self,
           bucket: &str,
           prefix: &str,
           continuation_token: Option<String>,
       ) -> Result<ListObjectsResult, Box<dyn Error + Send + Sync + 'static>> {
           let response = self
               .s3
               .list_objects_v2()
               .bucket(bucket)
               .prefix(prefix)
               .set_continuation_token(continuation_token)
               .send()
               .await?;
           Ok(ListObjectsResult {
               objects: response.contents().unwrap_or_default().to_vec(),
               continuation_token: response.continuation_token().map(|t| t.to_string()),
               has_more: response.is_truncated(),
           })
       }
   }
   ```

1. Create a fake implementation for use in unit tests:

   ```rust
   #[derive(Clone, Debug)]
   pub struct TestListObjects {
       expected_bucket: String,
       expected_prefix: String,
       pages: Vec<Vec<s3::model::Object>>,
   }
   
   #[async_trait]
   impl ListObjects for TestListObjects {
       async fn list_objects(
           &self,
           bucket: &str,
           prefix: &str,
           continuation_token: Option<String>,
       ) -> Result<ListObjectsResult, Box<dyn Error + Send + Sync + 'static>> {
           assert_eq!(self.expected_bucket, bucket);
           assert_eq!(self.expected_prefix, prefix);
   
           let index = continuation_token
               .map(|t| usize::from_str(&t).expect("valid token"))
               .unwrap_or_default();
           if self.pages.is_empty() {
               Ok(ListObjectsResult {
                   objects: Vec::new(),
                   continuation_token: None,
                   has_more: false,
               })
           } else {
               Ok(ListObjectsResult {
                   objects: self.pages[index].clone(),
                   continuation_token: Some(format!("{}", index + 1)),
                   has_more: index + 1 < self.pages.len(),
               })
           }
       }
   }
   ```

1. We can now give the test a list of pages of data to respond with in a unit test, which mimics the behavior of `ListObjectsV2`\. Here are our unit tests:

   ```rust
   #[tokio::test]
   async fn test_single_page() {
       use s3::model::Object;
   
       // Create a TestListObjects instance with just one page of two objects in it
       let fake = TestListObjects {
           expected_bucket: "some-bucket".into(),
           expected_prefix: "some-prefix".into(),
           pages: vec![[5, 2i64]
               .iter()
               .map(|size| Object::builder().size(*size).build())
               .collect()],
       };
   
       // Run the code we want to test with it
       let size = determine_prefix_file_size(&fake, "some-bucket", "some-prefix")
           .await
           .unwrap();
   
       // Verify we got the correct total size back
       assert_eq!(7, size);
   }
   
   #[tokio::test]
   async fn test_multiple_pages() {
       use s3::model::Object;
   
       // This time, we add a helper function for making pages
       fn make_page(sizes: &[i64]) -> Vec<Object> {
           sizes
               .iter()
               .map(|size| Object::builder().size(*size).build())
               .collect()
       }
   
       // Create the TestListObjects instance with two pages of objects now
       let fake = TestListObjects {
           expected_bucket: "some-bucket".into(),
           expected_prefix: "some-prefix".into(),
           pages: vec![make_page(&[5, 2]), make_page(&[3, 9])],
       };
   
       // And now test and verify
       let size = determine_prefix_file_size(&fake, "some-bucket", "some-prefix")
           .await
           .unwrap();
       assert_eq!(19, size);
   }
   ```



## Enums<a name="testing-2"></a>

This approach uses enums to unit test your code\.

For the enum approach, we create an enum to represent the `ListObjectsV2` call with two variants: `Real` and `Test`\.

1. Add the following code to exclude the `Test` variant from the production build:

   ```rust
   pub enum ListObjects {
       Real(s3::Client),
       #[cfg(test)]
       Test {
           expected_bucket: String,
           expected_prefix: String,
           pages: Vec<Vec<s3::model::Object>>,
       },
   }
   ```

1. With the enum in place, we can start implementing a list objects call\. Create a struct for the return value:

   ```rust
   pub struct ListObjectsResult {
       pub objects: Vec<s3::model::Object>,
       pub continuation_token: Option<String>,
       pub has_more: bool,
   }
   ```

1. Add the following list objects call that matches self to determine which implementation to use:

   ```rust
   impl ListObjects {
       pub async fn list_objects(
           &self,
           bucket: &str,
           prefix: &str,
           continuation_token: Option<String>,
       ) -> Result<ListObjectsResult, Box<dyn Error + Send + Sync + 'static>> {
           match self {
               Self::Real(s3) => {
                   Self::real_list_objects(s3.clone(), bucket, prefix, continuation_token).await
               }
               #[cfg(test)]
               Self::Test {
                   expected_bucket,
                   expected_prefix,
                   pages,
               } => {
                   assert_eq!(expected_bucket, bucket);
                   assert_eq!(expected_prefix, prefix);
                   Self::test_list_objects(pages, continuation_token)
               }
           }
       }
   ```

1. Next, implement `Real` and `Test`, which are called from the previous match\. Let’s start with the real implementation that wraps the Amazon S3 client and translates its output:

   ```rust
       async fn real_list_objects(
           s3: s3::Client,
           bucket: &str,
           prefix: &str,
           continuation_token: Option<String>,
       ) -> Result<ListObjectsResult, Box<dyn Error + Send + Sync + 'static>> {
           let response = s3
               .list_objects_v2()
               .bucket(bucket)
               .prefix(prefix)
               .set_continuation_token(continuation_token)
               .send()
               .await?;
           Ok(ListObjectsResult {
               objects: response.contents().unwrap_or_default().to_vec(),
               continuation_token: response.continuation_token().map(|t| t.to_string()),
               has_more: response.is_truncated(),
           })
       }
   ```

1. The test implementation mimics the Amazon S3 function’s behavior:

   ```rust
       #[cfg(test)]
       fn test_list_objects(
           pages: &[Vec<s3::model::Object>],
           continuation_token: Option<String>,
       ) -> Result<ListObjectsResult, Box<dyn Error + Send + Sync + 'static>> {
           use std::str::FromStr;
           let index = continuation_token
               .map(|t| usize::from_str(&t).expect("valid token"))
               .unwrap_or_default();
           if pages.is_empty() {
               Ok(ListObjectsResult {
                   objects: Vec::new(),
                   continuation_token: None,
                   has_more: false,
               })
           } else {
               Ok(ListObjectsResult {
                   objects: pages[index].clone(),
                   continuation_token: Some(format!("{}", index + 1)),
                   has_more: index + 1 < pages.len(),
               })
           }
       }
   }
   ```

1. Update the original implementation to use the enum instead of the Amazon S3 client:

   ```rust
   async fn determine_prefix_file_size(
       // Now we take an instance of our enum rather than the S3 client
       list_objects_impl: ListObjects,
       bucket: &str,
       prefix: &str,
   ) -> Result<usize, Box<dyn Error + Send + Sync + 'static>> {
       let mut next_token: Option<String> = None;
       let mut total_size_bytes = 0;
       loop {
           let result = list_objects_impl
               .list_objects(bucket, prefix, next_token.take())
               .await?;
   
           // Add up the file sizes we got back
           for object in result.objects {
               total_size_bytes += object.size() as usize;
           }
   
           // Handle pagination, and break the loop if there are no more pages
           next_token = result.continuation_token;
           if !result.has_more {
               break;
           }
       }
       Ok(total_size_bytes)
   }
   ```

1. Create the unit tests by instantiating the `ListObjects::Test` variant with some test data, and then test our `determine_prefix_file_size` function with the test data:

   ```rust
   #[tokio::test]
   async fn test_single_page() {
       use s3::model::Object;
   
       // Create a TestListObjects instance with just one page of two objects in it
       let fake = ListObjects::Test {
           expected_bucket: "some-bucket".into(),
           expected_prefix: "some-prefix".into(),
           pages: vec![[5, 2i64]
               .iter()
               .map(|size| Object::builder().size(*size).build())
               .collect()],
       };
   
       // Run the code we want to test with it
       let size = determine_prefix_file_size(fake, "some-bucket", "some-prefix")
           .await
           .unwrap();
   
       // Verify we got the correct total size back
       assert_eq!(7, size);
   }
   
   #[tokio::test]
   async fn test_multiple_pages() {
       use s3::model::Object;
   
       // This time, we add a helper function for making pages
       fn make_page(sizes: &[i64]) -> Vec<Object> {
           sizes
               .iter()
               .map(|size| Object::builder().size(*size).build())
               .collect()
       }
   
       // Create the TestListObjects instance with two pages of objects now
       let fake = ListObjects::Test {
           expected_bucket: "some-bucket".into(),
           expected_prefix: "some-prefix".into(),
           pages: vec![make_page(&[5, 2]), make_page(&[3, 9])],
       };
   
       // And now test and verify
       let size = determine_prefix_file_size(fake, "some-bucket", "some-prefix")
           .await
           .unwrap();
       assert_eq!(19, size);
   }
   ```

As you can see, there are a number of ways to structure your code so that it can be unit tested\. The approach you choose depends on what is most effective for your application\.