---
permalink: /blog/:path/
layout: blog-subpages
title: Official Memgraph Drivers Are Out!
description: We're happy to announce that Memgraph drivers for Python and C languages, as well as Memgraph CLI (command line interface) are now published.
date: 2019-05-31
sitemap: true
header-image: "https://github.com/memgraph/blog/blob/master/images/drivers.jpg?raw=true"
category: [Business]
---

# Official Memgraph Drivers Are Out!

We're happy to announce that Memgraph drivers for Python and C languages, as
well as Memgraph CLI (command line interface) are now published. You can find
their respective GitHub repositories here:

- [mgclient] (C driver)
- [pymgclient] (DB-API 2.0 compliant Python driver)
- [mgconsole] (command line interface)

## How to get started

We will now learn how to install [mgclient], [pymgclient] and [mgconsole] and
run a few basic queries.

### Installing mgclient

The Memgraph C driver, [mgclient], serves as a base for both [pymgclient] and
[mgconsole], so you will have to install it first. At this moment, the only way
to install [mgclient] is by building from source. You can start by cloning the
[GitHub repository](https://github.com/memgraph/mgclient) and following
installation instructions given in the README file.

### Installing pymgclient

After you have [mgclient] installed, the easiest way to install the Python
driver is by downloading it from PyPI using PIP:

```bash
$ pip3 install pymgclient
```

You can find more detailed installation instructions in the README file in
[pymgclient] GitHub repository or in the [pymgclient
documentation](https://memgraph.github.io/pymgclient).

### Installing mgconsole

The Memgraph command line interface, [mgconsole], also has to be installed from
source. The steps are very similar to those for installing the C driver. Again,
you can find the detailed instructions in the README file in [mgconsole] GitHub
repository.


### Using pymgclient

The Python driver is compliant with DB-API 2.0 specification, so using it
should feel natural if you have used other Python database drivers before. You
can find detailed documentation [here](https://memgraph.github.io/pymgclient/).
Here's a code snippet that shows how to connect to Memgraph and execute a
simple query:

```python
>>> import mgclient

# Make a connection to the database
>>> conn = mgclient.connect(host='127.0.0.1', port=7687)

# Create a cursor for query execution
>>> cursor = conn.cursor()

# Execute a query
>>> cursor.execute("""
        CREATE (n:Person {name: 'John'})-[e:KNOWS]->
               (m:Person {name: 'Steve'})
        RETURN n, e, m
    """)

# Fetch one row of query results
>>> row = cursor.fetchone()

>>> print(row[0])
(:Person {'name': 'John'})

>>> print(row[1])
[:KNOWS]

>>> print(row[2])
(:Person {'name': 'Steve'})

# Make database changes persistent
>>> conn.commit()
```

### Using mgconsole

Here's an example showing how to use the Memgraph command line interface:

```
$ mgconsole --host 127.0.0.1 --port 7687 --use-ssl=false
mgconsole 0.1
Type :help for shell usage
Quit the shell by typing Ctrl-D(eof) or :quit
Connected to 'memgraph://127.0.0.1:7687'
memgraph> :help
In interactive mode, user can enter cypher queries and supported commands.

Cypher queries can span through multiple lines and conclude with a
semi-colon (;). Each query is executed in the database and the results
are printed out.

The following interactive commands are supported:

        :help    Print out usage for interactive mode
        :quit    Exit the shell

memgraph>
memgraph> MATCH (t:Turtle) RETURN t;
+-------------------------------------------+
| t                                         |
+-------------------------------------------+
| (:Turtle {color: "blue", name: "Leo"})    |
| (:Turtle {color: "purple", name: "Don"})  |
| (:Turtle {color: "orange", name: "Mike"}) |
| (:Turtle {color: "red", name: "Raph"})    |
+-------------------------------------------+
4 rows in set (0.000 sec)
memgraph> :quit
Bye
```

---

We have now installed [mgclient], [pymgclient] and [mgconsole] and learned how
to run a few basic queries. You're now ready to explore on your own! Make sure
you check out the GitHub repositories for documentation and more resources.

The clients are still in early 0.1 version, so if you run into any issues or
have any improvement ideas, please feel free to raise an issue on GitHub or
submit a pull request. Any feedback is highly appreciated!

[mgclient]: https://github.com/memgraph/mgclient
[pymgclient]: https://github.com/memgraph/pymgclient
[mgconsole]: https://github.com/memgraph/mgconsole
