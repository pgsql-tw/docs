<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">clusterdb</span>

</div>

[Prev](reference-client.html "PostgreSQL Client Applications") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-createdb.html "createdb")

-----

<div id="APP-CLUSTERDB" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.3.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">clusterdb</span></span>

clusterdb — cluster a <span class="productname">PostgreSQL</span>
database

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`clusterdb` \[*`connection-option`*...\] \[ `--verbose` | `-v` \] \[
`--table` | `-t` *`table`* \] ... \[*`dbname`*\]

</div>

<div class="cmdsynopsis">

`clusterdb` \[*`connection-option`*...\] \[ `--verbose` | `-v` \]
`--all` | `-a`

</div>

</div>

<div id="id-1.9.4.3.5" class="refsect1">

## Description

<span class="application">clusterdb</span> is a utility for reclustering
tables in a <span class="productname">PostgreSQL</span> database. It
finds tables that have previously been clustered, and clusters them
again on the same index that was last used. Tables that have never been
clustered are not affected.

<span class="application">clusterdb</span> is a wrapper around the SQL
command
[<span class="refentrytitle">CLUSTER</span>](sql-cluster.html "CLUSTER").
There is no effective difference between clustering databases via this
utility and via other methods for accessing the server.

</div>

<div id="id-1.9.4.3.6" class="refsect1">

## Options

<span class="application">clusterdb</span> accepts the following
command-line arguments:

<div class="variablelist">

  - <span class="term">`-a`  
    </span><span class="term">`--all`</span>  
    Cluster all databases.

  - <span class="term">`[-d] dbname`  
    </span><span class="term">`[--dbname=]dbname`</span>  
    Specifies the name of the database to be clustered. If this is not
    specified and `-a` (or `--all`) is not used, the database name is
    read from the environment variable `PGDATABASE`. If that is not set,
    the user name specified for the connection is used.

  - <span class="term">`-e`  
    </span><span class="term">`--echo`</span>  
    Echo the commands that <span class="application">clusterdb</span>
    generates and sends to the server.

  - <span class="term">`-q`  
    </span><span class="term">`--quiet`</span>  
    Do not display progress messages.

  - <span class="term">`-t table`  
    </span><span class="term">`--table=table`</span>  
    Cluster *`table`* only. Multiple tables can be clustered by writing
    multiple `-t` switches.

  - <span class="term">`-v`  
    </span><span class="term">`--verbose`</span>  
    Print detailed information during processing.

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">clusterdb</span> version and
    exit.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">clusterdb</span> command
    line arguments, and exit.

</div>

<span class="application">clusterdb</span> also accepts the following
command-line arguments for connection parameters:

<div class="variablelist">

  - <span class="term">`-h host`  
    </span><span class="term">`--host=host`</span>  
    Specifies the host name of the machine on which the server is
    running. If the value begins with a slash, it is used as the
    directory for the Unix domain socket.

  - <span class="term">`-p port`  
    </span><span class="term">`--port=port`</span>  
    Specifies the TCP port or local Unix domain socket file extension on
    which the server is listening for connections.

  - <span class="term">`-U username`  
    </span><span class="term">`--username=username`</span>  
    User name to connect as.

  - <span class="term">`-w`  
    </span><span class="term">`--no-password`</span>  
    Never issue a password prompt. If the server requires password
    authentication and a password is not available by other means such
    as a `.pgpass` file, the connection attempt will fail. This option
    can be useful in batch jobs and scripts where no user is present to
    enter a password.

  - <span class="term">`-W`  
    </span><span class="term">`--password`</span>  
    Force <span class="application">clusterdb</span> to prompt for a
    password before connecting to a database.
    
    This option is never essential, since
    <span class="application">clusterdb</span> will automatically prompt
    for a password if the server demands password authentication.
    However, <span class="application">clusterdb</span> will waste a
    connection attempt finding out that the server wants a password. In
    some cases it is worth typing `-W` to avoid the extra connection
    attempt.

  - <span class="term">`--maintenance-db=dbname`</span>  
    Specifies the name of the database to connect to discover what other
    databases should be clustered. If not specified, the `postgres`
    database will be used, and if that does not exist, `template1` will
    be used.

</div>

</div>

<div id="id-1.9.4.3.7" class="refsect1">

## Environment

<div class="variablelist">

  - <span class="term">`PGDATABASE`  
    </span><span class="term">`PGHOST`  
    </span><span class="term">`PGPORT`  
    </span><span class="term">`PGUSER`</span>  
    Default connection parameters

</div>

This utility, like most other
<span class="productname">PostgreSQL</span> utilities, also uses the
environment variables supported by
<span class="application">libpq</span> (see
[Section 33.14](libpq-envars.html "33.14. Environment Variables")).

</div>

<div id="id-1.9.4.3.8" class="refsect1">

## Diagnostics

In case of difficulty, see
[<span class="refentrytitle">CLUSTER</span>](sql-cluster.html "CLUSTER")
and
[<span class="refentrytitle"><span class="application">psql</span></span>](app-psql.html "psql")
for discussions of potential problems and error messages. The database
server must be running at the targeted host. Also, any default
connection settings and environment variables used by the
<span class="application">libpq</span> front-end library will apply.

</div>

<div id="id-1.9.4.3.9" class="refsect1">

## Examples

To cluster the database `test`:

``` screen
$ clusterdb test
```

To cluster a single table `foo` in a database named
`xyzzy`:

``` screen
$ clusterdb --table=foo xyzzy
```

</div>

<div id="id-1.9.4.3.10" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle">CLUSTER</span>](sql-cluster.html "CLUSTER")</span>

</div>

</div>

<div class="navfooter">

-----

|                                |                             |                                           |
| :----------------------------- | :-------------------------: | ----------------------------------------: |
| [Prev](reference-client.html)  | [Up](reference-client.html) |                 [Next](app-createdb.html) |
| PostgreSQL Client Applications |     [Home](index.html)      | <span class="application">createdb</span> |

</div>
