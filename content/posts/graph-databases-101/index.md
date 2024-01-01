---
title: "Graph Databases 101: A Beginner’s Guide"
date: 2023-12-30
draft: false
tags: ["Graph Database", "AGE", "Hands-On", "Beginner"]
---

Welcome to my new post, continuing from the [last post](https://medium.com/@sharmavaibhav110028/the-three-ws-of-graph-databases-242ceac7ba69), I will briefly discuss some basic operations and an introduction to graph databases.

## Introduction
First, let’s talk about the language these databases understand. Most databases can understand more than one language, just as more than 60% of humans do. In our case, these are SQL and Cypher. Don’t worry if you don’t understand these languages, the syntax used in this tutorial is beginner-friendly and can be understood without prior experience.

## Getting Started
In the previous post, I set up our instance of Postgres and installed the AGE extension. Let’s now connect to the Postgres process through —

```bash
username=# CREATE EXTENSION age;
username=# LOAD 'age';
username=# SET search_path = ag_catalog, "$user", public;
```

The above three commands are used to connect with the Postgres server running in the background. The meaning of these three lines is mentioned below —

1. The first line loads an extension named `age`. It also creates new SQL objects.
2. The second line loads a shared library file (a.k.a the extension) in memory.
3. The third line sets an environment variable `search_path` to `ag_catalog`. This serves the same purpose as appending the schema name to all the functions you use from the AGE extension.

> Now that you have installed and loaded all the stuff, it’s time to learn some basic syntax about graph databases.

## Basic Coding

### Creating a graph

To create a graph, the following command can be inputted on the server —

```bash
username=# SELECT create_graph('scan');
```

> The above command creates a graph named “scan” containing no vertices and edges.

### Creating a vertex

The next thing to implement is the creation of vertices. Vertices can have labels. Labels are text associated with a vertex for conveying extra information about it. They are helpful in different conditions, like, representing vertices with the types of people based on income levels as labels. Let’s see a sample query and understand it thoroughly —

```bash
username=# SELECT * 
username=# FROM cypher('scan', $$
username=#   CREATE (field)
username=# $$) as (v agtype);
```

> Notice the multiple lines of the query. In the Postgres server, a query is not accepted until you end it with the delimiter “;”.

Let’s dissect the query in four lines and understand it one-by-one —

1. `SELECT *` is used to select the data in the database. “*” here represents everything.
2. `FROM cypher('scan', $$` is used to point to the graph that we are using. We use the cypher() function for the same. It takes two parameters — the graph name and the cypher query. Also, note that the cypher query starts and ends with $$.
3. `CREATE (field)` is used to create a vertex, where the name of the vertex is field . A quick tip here — Multiple vertices can be created through the modified query: CREATE (n), (m) .
4. `$$) AS (v agtype);` is used to return the result as agtype . Here the rows will be returned under the heading v .

> Note that, AGE only returns `agtype` as the return value.

The next task would be to create a vertex with a label on it. The query would change a little bit and is as follows —

```bash
username=# SELECT * 
username=# FROM cypher('scan', $$
username=#   CREATE (field:offside)
username=# $$) as (v agtype);
```

> Notice the “:offside” attached to the end of vertex name. That specifies the label.

Currently, the AGE project doesn’t support the definition of multiple labels for a single vertex. For that particular use case, we could use something called **property**.

### Creating properties

Properties are key-value pairs through which the vertices store information about them. Below is the example code for the same —

```bash
username=# SELECT * 
username=# FROM cypher('scan', $$
username=#   CREATE (field:offside { elevation: 1.5 })
username=# $$) as (v agtype);
```

> Notice the additional “{ elevation: 1.5 }” part. That part specifies the properties associated with the vertex. We can add additional properties by just appending and comma-separating them within the brackets.

The last thing to cover in this introductory guide is how to display the graph / visualize it.

### Displaying the graph

Below is the query to display the graph. Don’t worry if you don’t understand it, I will further dissect it and make you understand.

```bash
username=# SELECT * FROM cypher('scan', $$
username=#   MATCH (v) RETURN v
username=# $$) as (v agtype);
```

The first and last statements are already explained above. I will explain the second statement below —

`MATCH(v) RETURN v` returns all the nodes in the database.

> The `MATCH()` function actually matches a pattern. In this pattern, we defined only node names and not any labels, which in turn will return all the nodes.

## Conclusion

That’s it for this tutorial! I know I have skipped some parts of the basic coding, but I will cover it in the next post, so stay tuned.

Until then, see ya!

{{< swatches "#FB6962" "#79DE79" "#99C3EC" >}}
