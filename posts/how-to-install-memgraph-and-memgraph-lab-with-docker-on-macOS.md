---
permalink: /blog/:path/
layout: blog-subpages

title: How to Install Memgraph and Memgraph Lab with Docker on macOS
description: A tutorial for installing Memgraph and Memgraph Lab on macOS
date: 2019-01-21
status: published
header-image: "https://i.imgur.com/T7Ilj3h.jpg"

category: [Tutorial]

sitemap: true
---
# How to Install Memgraph and Memgraph Lab with Docker on macOS

### Introduction

[Memgraph](https://memgraph.com/product/) is a native, in-memory graph database built for real-time business-critical applications. Memgraph supports strongly-consistent ACID transactions; and supports the Cypher query language for structuring, manipulating, and exploring data.

[Memgraph Lab](https://memgraph.com/product/lab/) is a lightweight and intuitive Cypher and [Bolt](https://boltprotocol.org/) compatible integrated development environment (IDE). It's designed to help you import data, develop, debug, and profile database queries and visualize query results.

In this tutorial, you will install both Memgraph and Memgraph Lab on your Mac. You will then test each installation by running basic Cypher queries to make sure that everything is working correctly.

## Prerequisites

For a seamless installation of Memgraph and Memgraph Lab on your Mac, ensure that you have:
* A computer running macOS version 10.13 (High Sierra) or higher.
* Administrative rights to your macOS and an internet connection.
* A [Docker Hub](https://hub.docker.com/) account, so that you can download the [Docker Desktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac) version 1.12 or higher and install it on your computer.
* Basic knowledge of working with the command line.

## Step 1 — Downloading and Installing Memgraph

You can install Memgraph on macOS using the [Docker](https://www.docker.com/) image provided on Memgraph's website. [Download and save it](https://memgraph.com/download/memgraph/v0.14.1/memgraph-0.14.1-docker.tar.gz) to a location you can quickly locate later on.

Before we go on, it's essential that you [have Docker installed on your system](https://docs.docker.com/docker-for-mac/install/) so you are able to install and run Memgraph.

Once you have Docker installed and the Memgraph Docker image downloaded, run the command below in your command line to begin the installation. Simply replace the `<version>` placeholder with the version number included in the filename of the Docker image you've just downloaded:

```
docker load -i /path/to/memgraph-<version>-docker.tar.gz
```

<p id="run-memgraph-server">Now, start the Memgraph server by issuing this command:</p>

```
docker run -p 7687:7687 -v mg_lib:/var/lib/memgraph -v mg_log:/var/log/memgraph -v mg_etc:/etc/memgraph memgraph
```

If successful, you should be greeted with the following message:

```
Starting 2 BoltS workers
BoltS server is fully armed and operational
BoltS listening on 0.0.0.0:7687
```

The server is now ready to process your queries.

If you want to stop the server, press `Ctrl + C` or `Cmd + .`. If you do, make sure you run it back again using the previous command. The server needs to be running to execute the next step.

You now have Memgraph installed and running. The next step is to play around by executing some queries against it using the CLI (command-line interface) tool.

## Step 2 — Querying Memgraph Through the CLI

Memgraph comes with a CLI tool named `mg_client`. It allows you to execute Cypher queries against the Memgraph server using the command line.

Memgraph supports the Cypher query through the  [openCypher](https://www.opencypher.org/) project. Cypher is a SQL-like declarative query language purposely designed to efficiently query and update property graphs.

We'll now test Memgraph by executing two basic queries that demonstrate the creation and retrieval of data from the database.

But before we do that, we first need to retrieve the IP address of our Docker container that runs the Memgraph server. The `mg_client` tool needs the IP address of the server to connect and run queries against it.

To do this, first, open a new window or tab in your command line. The window or tab you used to run the Memgraph server can't execute another command. Simply run this command on a new window or tab:

```
docker ps
```

You should then see an output similar to the following:

```
CONTAINER ID        IMAGE           COMMAND                  CREATED         ...
d1d9d4ce649f        memgraph        "/usr/lib/memgraph/m…"   2 seconds ago   ...
```

The value we're interested in is the one in the `CONTAINER ID` column. In the example output above, it's `d1d9d4ce649f`.

Now, use the container ID to retrieve the IP address of the container. Run the command below, at the same time, replacing `CONTAINER_ID` with your container ID:

```
docker inspect -f '' CONTAINER_ID
```

The command above will return a long JSON string that contains various properties that describe the container. Here's a truncated version of it:

```
[
  {
    "Id": "d1d9d4ce649fc5d78dee28eb946ac6f5b352f6ef1ee05eb6518bdec4061a55ef",
    .......
    "NetworkSettings": {
        "Gateway": "172.17.0.1",
        "IPAddress": "172.17.0.2",
        ......
    }
  }
]
```

Inside the JSON string, look for the value of the key named `IPAddress` as this is the IP address we are looking for. Yours will vary, but, in our case, it's `172.17.0.2`.

Next, run the `mg_client` with this command, and at the same time, correctly replacing `HOST` with your IP address:

```
docker run -it --entrypoint=mg_client memgraph --host HOST
```

If the `mg_client` can connect to the server, you'll receive a prompt similar to this:

```
mg_client 0.14.1
Type :help for shell usage
Quit the shell by typing Ctrl-D(eof) or :quit
Connected to 'memgraph://172.17.0.2:7687'
memgraph>
```

You are now ready to execute Cypher queries on the server. Each query needs to end with the `;` (_semicolon_) character.

<p id="first-query">Let's begin by executing this first query:</p>

```
CREATE (u:User {name: "Alice"})-[:Likes]->(m:Software {name: "Memgraph"});
```
<p id="query-explain">The above will create 2 nodes in the database, one labeled "User" with the name "Alice" and the other labeled "Software" with the name "Memgraph". It will also create a relationship that "Alice" likes "Memgraph".</p>

Next, find the created nodes and relationships by executing this second query:

```
MATCH (u:User)-[r]->(x) RETURN u, r, x;
```

The query returns the following result:
```
+--------------------------------+--------------------------------+--------------------------------+

| u  | r  | x  |

+--------------------------------+--------------------------------+--------------------------------+

| (:User {name: "Alice"})  | [Likes]  | (:Software {name: "Memgraph"}) |

+--------------------------------+--------------------------------+--------------------------------+

1 row in set (0.038 sec)
```

Lastly, type `:quit` in the prompt to exit the `mg_client` tool.

Excellent! You have now installed Memgraph and used its CLI tool to run a simple query.

Now, aside from a CLI tool, Memgraph also has an integrated development environment called Memgraph Lab to help you manage and execute various tasks on your databases. Let's move on to the next step to install it.

## Step 3 — Installing Memgraph Lab and Connecting to Memgraph

Start by selecting and downloading the Memgraph Lab build for macOS from Memgraph's website [Memgraph Lab Download page](https://memgraph.com/download/).


Once downloaded, installing the app on your system is similar to how you would install most Mac apps — by opening the `.dmg` file and dragging the app (Memgraph Lab) into the `/Applications` folder.


After installing the app, run it.

Now, depending on your macOS' security preferences, it might block the app from running the first time. This is because it can't recognize the developer of the app, which in this case, is Memgraph.


This is normal and nothing you should worry about. Just press the "OK" button to dismiss the dialog box. 

What you need to do next is to allow macOS to run Memgraph Lab. To do this, [open your system's System Preferences](https://support.apple.com/guide/mac-help/set-system-preferences-mh15217/10.13/mac/10.13) and then click Security & Privacy preferences.

Now, inside the Security & Privacy preference pane, ensure that the "General" tab is active and look to the "Allow apps downloaded from:" section. Any option or setting that will allow Memgraph Lab to run will be available there.


For more information about allowing unidentified apps to run on macOS, see the articles [Open an app from an unidentified developer](https://support.apple.com/kb/ph25088?locale=en_US) and the [Safely open apps on your Mac](https://support.apple.com/en-us/HT202491).

After updating your security preferences, try rerunning Memgraph Lab. Just click "Open" if macOS asks you if you really want to open it. This is again part of macOS' security precautions.


Memgraph Lab will now open, and you should be presented with the Home screen. Using the default values of the "Host" and "Port" text fields, click "Connect" to start connecting to the Memgraph server. In the meantime, just leave the "Username" and "Password" fields blank.


Before connecting, [ensure that the Memgraph server is running](#run-memgraph-server) as explained on Step 1. You won't be able to connect if the server is not running in the first place!

Once connected, you'll be presented with Memgraph Lab's user interface.


Now that we have Memgraph Lab installed and connected to Memgraph, we will run a few basic queries to make sure everything works properly.

## Step 4 — Testing the Memgraph Lab Installation

Let's test Memgraph Lab by running [the same queries we ran in Step 2](#first-query).

 1. First, click the "Query" tab on the left sidebar.
 2. Next, enter this first query at the query editor which is located at the top of the screen:
    ```
    CREATE (u:User {name: "Alice"})-[:Likes]->(m:Software {name: "Memgraph"});
    ```
    The query above [creates 2 nodes and a relationship between them](#first-query).

 3. Lastly, click "Run" or press `Cmd + Enter` to execute the query.

If no error message appeared, that means your query executed successfully. Again, this is the [same query we ran in Step 2](#first-query). Kindly refer back to it for its explanation.

We will now retrieve the nodes and relationships we've just created by executing the following Cypher query:

```
MATCH (u:User)-[r]->(x) RETURN u, r, x;
```

You should get a result that looks similar to this:

![Memgraph Lab displaying the graph visualization of a query result.](https://i.imgur.com/tSmbKmy.png)


You now have Memgraph Lab working correctly on your system. Memgraph Lab's visual presentation of query results is one of its best features. It helps you understand the relationships of your nodes compared to the tabular format presented by the `mg_client` tool we introduced in Step 2. This makes Memgraph Lab the preferred tool for both beginners learning Memgraph and the Cypher query language, and advanced Memgraph users working on complex queries.

## Conclusion

In this tutorial, you installed Memgraph and Memgraph Lab on macOS. You tested the Memgraph installation by running a Memgraph server and using the `mg_client` CLI tool to execute Cypher queries against the server. The same goes for Memgraph Lab, but instead of using the CLI tool, you ran the queries inside the application.

If you are interested in learning more about each product, please consult the respective [Memgraph](https://memgraph.com/product/) and [Memgraph Lab](https://memgraph.com/product/lab/).
