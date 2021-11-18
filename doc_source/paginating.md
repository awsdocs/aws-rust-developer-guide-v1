--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Paginating in the AWS SDK for Rust<a name="paginating"></a>

This section describes how to handle cases where an API does not return all of the available objects\.

If you went through the [Create your first SDK app](getting-started.md#hello-world) topic, you created a Rust app that lists up to 10 objects in an Amazon S3 bucket\. How do we know whether the bucket has more than 10 objects? If there are more objects, how do we list the remaining objects? Let's start with answering the first question\.

Add the following code after the loop that displays the object names in the **hello\_world** code example to the following:

```
if resp.is_truncated {
    println!("There are more objects!");
}
```

Run the program again\. If you see the *There are more objects\!* in the output, you know there are more objects\. Let's modify the code to display the remaining objects\.

Replace everything after creating the client with the following code\. Don't forget to replace *yourbucketname* with the name of your bucket\. Note that it prints the message `–- more –-` after every 10 objects:

```
let resp = client
    .list_objects_v2()
    .bucket("yourbucketname")
    .max_keys(10)
    .send()
    .await?;

for object in resp.contents.unwrap_or_default() {
    println!("{}", object.key.as_deref().unwrap_or_default());
}

let mut truncated = resp.is_truncated;
let mut token = resp.next_continuation_token.expect("truncated response should have token");

while truncated {
    println!("--- more ---");
    
    // We list 10 objects at a time.
    let resp = client
        .list_objects_v2()
        .bucket("yourbucketname")
        .max_keys(10)
        .continuation_token(token.unwrap_or_default())
        .send()
        .await?;

    for object in resp.contents.unwrap_or_default() {
        println!("{}", object.key.as_deref().unwrap_or_default());
    }

    truncated = resp.is_truncated;
    token = resp.next_continuation_token.expect("truncated response should have token");
}

Ok(())
```