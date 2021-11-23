--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Paginating in the AWS SDK for Rust<a name="paginating"></a>

This section describes how to handle cases where an API does not return all of the available objects in a request\.

If you went through the [Create your first SDK app](getting-started.md#hello-world) topic, you created a Rust app that lists all of your tables in a region\.

Let's modify that app to only list the first 10 tables, then show you how to detect whether there are more than 10 tables\. And finally, if there are more tables, how to list the remaining tables\. Here's the code for listing only the first 10 tables\.

```
use aws_config::meta::region::RegionProviderChain;
use aws_dynamodb::{Client, Error};

#[tokio::main]
async fn main() -> Result<(), Error> {
    let region_provider = RegionProviderChain::default_provider().or_else("us-east-1");
    let config = aws_config::from_env().region(region_provider).load().await;
    let client = Client::new(&config);

    let resp = client.list_tables().limit(10).send().await?;

    println!("Tables:");

    let names = resp.table_names.unwrap_or_default();
    let len = names.len();

    for name in names {
        println!("  {}", name);
    }

    println!("Found {} tables", len);

    Ok(())
}
```

Add the following code after the loop that displays the table names in the **hello\_world** code example to the following:

```
if resp.last_evaluated_table_name != None {
    println!("There are more tables!");
}
```

Run the program again\. If you see the *There are more tables\!* in the output, you know there are more tables\. Let's modify the code to display the remaining tables\.

Replace everything after creating the client with the following code\. Note that it prints the message `–- more –-` after every 10 tables:

```
if resp.last_evaluated_table_name != None {
    println!("-- more --");
    let resp = client
        .list_tables()
        .limit(10)
        .exclusive_start_table_name(
            resp.last_evaluated_table_name
                .as_deref()
                .unwrap_or_default(),
        )
        .send()
        .await?;

    let names = resp.table_names.unwrap_or_default();
    let len = names.len();

    for name in names {
        println!("  {}", name);
    }

    num_tables += len;
}

println!("Found {} tables", num_tables);

Ok(())
```