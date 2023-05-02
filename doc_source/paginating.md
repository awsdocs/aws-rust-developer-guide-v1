--------

 *This is documentation for the developer preview release of the AWS SDK for Rust\. Do not use it in production as it is subject to breaking changes\.* 

--------

# Paginating in the AWS SDK for Rust<a name="paginating"></a>

This section describes how to handle cases where an API does not return all of the available objects in a request\.

If you went through the [Create your first SDK app](getting-started.md#hello-world) topic, you created a Rust app that lists all of your tables in a region\.

Let's modify that app to only list the first 10 tables, then show you how to detect whether there are more than 10 tables\. And finally, if there are more tables, how to list the remaining tables\. To list only the first 10 tables change the call to `list_tables` to the following\.

```
    let resp = client.list_tables().limit(10).send().await?;
```

Now when you run the program it only shows up to the first 10 tables\.

Let's add a bit of code to detect when we have more than 10 tables\. Add the following code after the print statement that displays how many table names it found\.

```
    if resp.last_evaluated_table_name.is_some() {
        println!("There are more tables");
    }
```

Run the program again\. If you see the *There are more tables\!* in the output, you know there are more tables\. Let's modify the code to display the remaining tables\.

Replace everything after creating the client with the following code\. Note that it prints the message `–- more –-` after every 10 tables:

```
    let mut resp = client.list_tables().limit(10).send().await?;
    let mut names = resp.table_names.unwrap_or_default();
    let len = names.len();

    let mut num_tables = len;

    println!("Tables:");

    for name in &names {
        println!("  {}", name);
    }

    while resp.last_evaluated_table_name.is_some() {
        println!("-- more --");
        resp = client
            .list_tables()
            .limit(10)
            .exclusive_start_table_name(
                resp.last_evaluated_table_name
                    .as_deref()
                    .unwrap_or_default(),
            )
            .send()
            .await?;

        let mut more_names = resp.table_names.unwrap_or_default();
        num_tables += more_names.len();

        for name in &more_names {
            println!("  {}", name);
        }
        names.append(&mut more_names);
    }

    println!();
    println!("Found {} tables", num_tables);

    Ok(names)
```