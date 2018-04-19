<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">reindexdb</span>

</div>

[Prev](app-psql.html "psql") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-vacuumdb.html "vacuumdb")

-----

<div id="APP-REINDEXDB" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.19.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">reindexdb</span></span>

reindexdb — reindex a <span class="productname">PostgreSQL</span>
database

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`reindexdb` \[*`connection-option`*...\] \[*`option`*...\] \[ `--schema`
| `-S` *`schema`* \] ... \[ `--table` | `-t` *`table`* \] ... \[
`--index` | `-i` *`index`* \] ... \[*`dbname`*\]

</div>

<div class="cmdsynopsis">

`reindexdb` \[*`connection-option`*...\] \[*`option`*...\] `--all` |
`-a`

</div>

<div class="cmdsynopsis">

`reindexdb` \[*`connection-option`*...\] \[*`option`*...\] `--system` |
`-s` \[*`dbname`*\]

</div>

</div>

<div id="id-1.9.4.19.5" class="refsect1">

## Description

<span class="application">reindexdb</span> is a utility for rebuilding
indexes in a <span class="productname">PostgreSQL</span> database.

<span class="application">reindexdb</span> is a wrapper around the SQL
command
[<span class="refentrytitle">REINDEX</span>](sql-reindex.html "REINDEX").
There is no effective difference between reindexing databases via this
utility and via other methods for accessing the server.

</div>

<div id="id-1.9.4.19.6" class="refsect1">

## Options

<span class="application">reindexdb</span> accepts the following
command-line arguments:

<div class="variablelist">

  - <span class="term">`-a`  
    </span><span class="term">`--all`</span>  
    Reindex all databases.

  - <span class="term">`[-d] dbname`  
    </span><span class="term">`[--dbname=]dbname`</span>  
    Specifies the name of the database to be reindexed. If this is not
    specified and `-a` (or `--all`) is not used, the database name is
    read from the environment variable `PGDATABASE`. If that is not set,
    the user name specified for the connection is used.

  - <span class="term">`-e`  
    </span><span class="term">`--echo`</span>  
    Echo the commands that <span class="application">reindexdb</span>
    generates and sends to the server.

  - <span class="term">`-i index`  
    </span><span class="term">`--index=index`</span>  
    Recreate *`index`* only. Multiple indexes can be recreated by
    writing multiple `-i` switches.

  - <span class="term">`-q`  
    </span><span class="term">`--quiet`</span>  
    Do not display progress messages.

  - <span class="term">`-s`  
    </span><span class="term">`--system`</span>  
    Reindex database's system catalogs.

  - <span class="term">`-S schema`  
    </span><span class="term">`--schema=schema`</span>  
    Reindex *`schema`* only. Multiple schemas can be reindexed by
    writing multiple `-S` switches.

  - <span class="term">`-t table`  
    </span><span class="term">`--table=table`</span>  
    Reindex *`table`* only. Multiple tables can be reindexed by writing
    multiple `-t` switches.

  - <span class="term">`-v`  
    </span><span class="term">`--verbose`</span>  
    Print detailed information during processing.

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">reindexdb</span> version and
    exit.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">reindexdb</span> command
    line arguments, and exit.

</div>

<span class="application">reindexdb</span> also accepts the following
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
    Force <span class="application">reindexdb</span> to prompt for a
    password before connecting to a database.
    
    This option is never essential, since
    <span class="application">reindexdb</span> will automatically prompt
    for a password if the server demands password authentication.
    However, <span class="application">reindexdb</span> will waste a
    connection attempt finding out that the server wants a password. In
    some cases it is worth typing `-W` to avoid the extra connection
    attempt.

  - <span class="term">`--maintenance-db=dbname`</span>  
    Specifies the name of the database to connect to discover what other
    databases should be reindexed. If not specified, the `postgres`
    database will be used, and if that does not exist, `template1` will
    be used.

</div>

</div>

<div id="id-1.9.4.19.7" class="refsect1">

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

<div id="id-1.9.4.19.8" class="refsect1">

## Diagnostics

In case of difficulty, see
[<span class="refentrytitle">REINDEX</span>](sql-reindex.html "REINDEX")
and
[<span class="refentrytitle"><span class="application">psql</span></span>](app-psql.html "psql")
for discussions of potential problems and error messages. The database
server must be running at the targeted host. Also, any default
connection settings and environment variables used by the
<span class="application">libpq</span> front-end library will apply.

</div>

<div id="id-1.9.4.19.9" class="refsect1">

## Notes

<span class="application">reindexdb</span> might need to connect several
times to the <span class="productname">PostgreSQL</span> server, asking
for a password each time. It is convenient to have a `~/.pgpass` file in
such cases. See
[Section 33.15](libpq-pgpass.html "33.15. The Password File") for more
information.

</div>

<div id="id-1.9.4.19.10" class="refsect1">

## Examples

To reindex the database `test`:

``` screen
$ reindexdb test
```

To reindex the table `foo` and the index `bar` in a database named
`abcd`:

``` screen
$ reindexdb --table=foo --index=bar abcd
```

</div>

<div id="id-1.9.4.19.11" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle">REINDEX</span>](sql-reindex.html "REINDEX")</span>

</div>

</div>

<div class="navfooter">

-----

|                                       |                             |                                           |
| :------------------------------------ | :-------------------------: | ----------------------------------------: |
| [Prev](app-psql.html)                 | [Up](reference-client.html) |                 [Next](app-vacuumdb.html) |
| <span class="application">psql</span> |     [Home](index.html)      | <span class="application">vacuumdb</span> |

</div>
