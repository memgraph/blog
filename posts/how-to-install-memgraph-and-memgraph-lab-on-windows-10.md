---
permalink: /blog/:path/
layout: blog-subpages

title: How to Install Memgraph and Memgraph Lab on Windows 10
description: A tutorial for installing Memgraph and Memgraph Lab on Windows 10
date: 2020-01-17
status: published
header-image: "https://i.imgur.com/BqG6r2D.jpg"
category: [Tutorial]

sitemap: true
---

# How to Install Memgraph and Memgraph Lab on Windows 10.
 
[Memgraph](https://memgraph.com/product/) is a native, in-memory graph database built for real-time business-critical applications. Memgraph supports strongly-consistent ACID transactions; and uses the [Cypher query language](https://www.opencypher.org/) for structuring, manipulating, and exploring data.
 
[Memgraph Lab](https://memgraph.com/product/lab/) is a lightweight and intuitive Cypher and [Bolt](https://boltprotocol.org/) compatible integrated development environment (IDE). It's designed to help you import data, develop, debug, and profile database queries and visualize query results.
 
In this tutorial, you will install both Memgraph and Memgraph Lab on your Windows 10 PC. You will then test each installation by running a few basic queries to make sure that everything is working correctly.
 
## Prerequisites
 
For a seamless installation of Memgraph and Memgraph Lab on your Windows PC, ensure that you have:
* A computer running Windows 10 (64-bit version) with [Fall Creators Update](https://support.microsoft.com/en-us/help/4028685/windows-10-get-the-update).
* Administrative rights to your Windows PC and an internet connection.
* Basic knowledge of working with the command line.
 
## Step 1 — Enable Windows Subsystem for Linux
 
The Windows subsystem for Linux became a stable feature in [Fall Creators Update](https://support.microsoft.com/en-us/help/4028685/windows-10-get-the-update) of Windows 10. You can now run Ubuntu, Debian or Kali right from your Windows machine. It is only available for 64 bit Windows so make sure you are running the 64-bit version of Windows. You can determine whether your computer is running a 32-bit version or 64-bit version of the Windows operating system by following [this guide](https://support.microsoft.com/en-us/help/827218/how-to-determine-whether-a-computer-is-running-a-32-bit-version-or-64).
 
Once you have made sure you are running the 64-bit version of Windows, you need to enable the Windows subsystem for Linux. To fo this, you can follow [these instructions](https://docs.microsoft.com/en-us/windows/wsl/install-win10). Please make sure to install the **Debian** distribution of Linux from the Windows App store.
 
Once your installation is complete, you can test it by clicking on the **start** button on the bottom left side of your Windows screen, searching for **Debian** and launching the app.
 
![Debian Launcher in Windows Start](https://i.imgur.com/NS8e0T9.png)
 
The first time you launch the app, you will be asked to enter your username and password. If everything works properly, you will get the following output:
 
![Debian shell running in Windows](https://i.imgur.com/6XYwxg5.png)
 
Congratulations! You have successfully installed the Debian distribution of Linux on your Windows machine. You are now ready to install Memgraph.
## Step 2 — Installing Memgraph
 
To get started, go to Memgraph's [downolad](https://memgraph.com/download/) page and download the latest Debian installation package. 
 
Once the download is complete, open your Debian shell and run the following command to start the installation process:
```
dpkg -i /path/to/memgraph_<version>.deb
```
* replace `/path/to` with path where you downloaded your installation package.
* replace `_version` with the version of the package that you are installing(usually the name of the installation package you downloaded). 
 
For example, if you downloaded version `0.50.0-1_amd64` to your D: Directory than this command will be changed to:
```
dpkg -i /mnt/d/memgraph_0.50.0-1_amd64.deb
```
Note: If you see any error related to missing dependency packages. You might have to run `sudo apt get update` before installing Memgraph.
 
<p id="run-memgraph-server">Now, start the Memgraph server by issuing the following command:</p>
 
```
/usr/lib/memgraph/memgraph
```
If Memgraph has been installed correctly you will see something like this:
> Starting 4 BoltS workers
> BoltS server is fully armed and operational
> BoltS listening on 0.0.0.0:7687
 
Awesome! You now have a running instance of Memgraph on your Windows machine. In the next step, you will learn how to query Memgraph from the command line interface (CLI).
 
## Step 3 — Querying Memgraph through the CLI tool with mgconsol.
 
Memgraph comes with a tool called `mgconsole` which allows you to execute queries against the Memgraph server from your command-line interface.
 
To install `mgconsole`, you can refer to the [mgconcole installation guide](https://github.com/memgraph/mgconsole)
 
Once you have `mgconsole` successfully installed, you are now ready to query Memgraph. To make sure that everything works, we will execute two basic queries that demonstrate the creation and retrieval of data from the database.
 
Memgraph supports the Cypher query through the  [openCypher](https://www.opencypher.org/) project. Cypher is a SQL-like declarative query language purposely designed to efficiently query and update property graphs.
 
 
<p id="first-query">Let's begin by executing a first query to create a small dataset:</p>
 
```
CREATE (u:User {name: "Alice"})-[:Likes]->(m:Software {name: "Memgraph"});
```
<p id="query-explain">The above will create 2 nodes in the database, one labeled "User" with the name "Alice" and the other labeled "Software" with the name "Memgraph". It will also create a relationship that "Alice" likes "Memgraph".</p>
 
Note: Each query needs to end with the `;` (_semicolon_) character.
 
Next, we will retrieve the created nodes and relationships by executing this second query:
 
```
MATCH (u:User)-[r]->(x) RETURN u, r, x;
```
 
The query should return the following result:
```
+--------------------------+----------+--------------------------------+
|         u                |     r    |                x               |
+--------------------------+----------+--------------------------------+
| (:User {name: "Alice"})  | [Likes]  | (:Software {name: "Memgraph"}) |
+--------------------------+----------+--------------------------------+
```

Excellent! You have now installed Memgraph and used its CLI tool to run a few simple queries.
 
Now, aside from a CLI tool, Memgraph also has an integrated development environment called Memgraph Lab to help you manage and execute various tasks on your databases. Let's move on to the next step to install it.
 
## Step 4 — Installing Memgraph Lab and Connecting to Memgraph
 
Start by downloading [Memgraph Lab](https://memgraph.com/download/) for Windows.
 
The downloaded package will be a `.exe` installer and can be easily installed just like other Windows installers.
 
Double click on the installer to start the installation process. Depending on your Windows security preferences, Windows Defender SmartScreen might block the installer from running the first time. This is because it can’t recognize the developer of the app, which in this case, is Memgraph. This is normal and nothing you should worry about. Simply click on **More Info** and then click on **Run Anyway**.
 
 
Once the installation is completed, Memgraph Lab will launch, and you will be presented with the Home screen. Using the default values of the "Host" and "Port" text fields and leaving the "Username" and "Password" fields blank click "Connect" to start connecting to the Memgraph server.
 
 
Note: Before connecting, [ensure that the Memgraph server is running](#run-memgraph-server) as explained on Step 2. You won't be able to connect if the server is not running in the first place!
 
Once connected, you'll be presented with Memgraph Lab's user interface.
 
 
Now that we have Memgraph Lab installed and connected to Memgraph, we will run a few basic queries to make sure everything works properly.
 
## Step 5 — Testing the Memgraph Lab Installation
 
Let's test Memgraph Lab by running [the same queries we ran in Step 2](#first-query).
 
 1. First, click the "Query" tab on the left sidebar.
 2. Next, enter this first query at the query editor which is located at the top of the screen:
    ```
    CREATE (u:User {name: "Alice"})-[:Likes]->(m:Software {name: "Memgraph"});
    ```
    The query above creates 2 nodes and a relationship between them.
 
 3. Lastly, click "Run" or press `Ctrl + Enter` to execute the query.
 
 
If no error message appeared, that means your query executed successfully. Again, this is the [same query we ran in Step 2](#first-query). Kindly refer back to it for its explanation.
 
We will now retrieve the nodes and relationships we've just created by executing the following Cypher query:
 
```
MATCH (u:User)-[r]->(x) RETURN u, r, x;
```
 
You should get a result that looks similar to this in the graph tab:
 
![Memgraph Lab displaying the graph visualization of a query result as Graph.](https://i.imgur.com/MghZbgi.png)
 
You now have Memgraph Lab working correctly on your system. Memgraph Lab's visual presentation of query results is one of its best features. It helps you understand the relationships of your nodes compared to the tabular format presented by the `mgconsole` tool we introduced in Step 3. This makes Memgraph Lab the preferred tool for both beginners learning Memgraph and the Cypher query language, and advanced Memgraph users working on complex queries.
 
## Conclusion
 
In this tutorial, you installed Memgraph and Memgraph Lab on Windows 10 using Windows Subsystem for Linux. You tested the Memgraph installation by running a Memgraph server and using the `mgconsole` CLI tool to execute Cypher queries. The same goes for Memgraph Lab, but instead of using the CLI tool, you ran the queries inside the application.
