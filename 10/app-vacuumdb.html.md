<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">vacuumdb</span>

</div>

[Prev](app-reindexdb.html "reindexdb") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](reference-server.html "PostgreSQL Server Applications")

-----

<div id="APP-VACUUMDB" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.20.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">vacuumdb</span></span>

vacuumdb — garbage-collect and analyze a
<span class="productname">PostgreSQL</span> database

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`vacuumdb` \[*`connection-option`*...\] \[*`option`*...\] \[ `--table` |
`-t` *`table`* \[( *`column`* \[,...\] )\] \] ... \[*`dbname`*\]

</div>

<div class="cmdsynopsis">

`vacuumdb` \[*`connection-option`*...\] \[*`option`*...\] `--all` | `-a`

</div>

</div>

<div id="id-1.9.4.20.5" class="refsect1">

## Description

<span class="application">vacuumdb</span> is a utility for cleaning a
<span class="productname">PostgreSQL</span> database.
<span class="application">vacuumdb</span> will also generate internal
statistics used by the <span class="productname">PostgreSQL</span> query
optimizer.

<span class="application">vacuumdb</span> is a wrapper around the SQL
command
[<span class="refentrytitle">VACUUM</span>](sql-vacuum.html "VACUUM").
There is no effective difference between vacuuming and analyzing
databases via this utility and via other methods for accessing the
server.

</div>

<div id="id-1.9.4.20.6" class="refsect1">

## Options

<span class="application">vacuumdb</span> accepts the following
command-line arguments:

<div class="variablelist">

  - <span class="term">`-a`  
    </span><span class="term">`--all`</span>  
    Vacuum all databases.

  - <span class="term">`[-d] dbname`  
    </span><span class="term">`[--dbname=]dbname`</span>  
    Specifies the name of the database to be cleaned or analyzed. If
    this is not specified and `-a` (or `--all`) is not used, the
    database name is read from the environment variable `PGDATABASE`. If
    that is not set, the user name specified for the connection is used.

  - <span class="term">`-e`  
    </span><span class="term">`--echo`</span>  
    Echo the commands that <span class="application">vacuumdb</span>
    generates and sends to the server.

  - <span class="term">`-f`  
    </span><span class="term">`--full`</span>  
    Perform <span class="quote">“<span class="quote">full</span>”</span>
    vacuuming.

  - <span class="term">`-F`  
    </span><span class="term">`--freeze`</span>  
    Aggressively
    <span class="quote">“<span class="quote">freeze</span>”</span>
    tuples.

  - <span class="term">`-j njobs`  
    </span><span class="term">`--jobs=njobs`</span>  
    Execute the vacuum or analyze commands in parallel by running
    *`njobs`* commands simultaneously. This option reduces the time of
    the processing but it also increases the load on the database
    server.
    
    <span class="application">vacuumdb</span> will open *`njobs`*
    connections to the database, so make sure your
    [max\_connections](runtime-config-connection.html#GUC-MAX-CONNECTIONS)
    setting is high enough to accommodate all connections.
    
    Note that using this mode together with the `-f` (`FULL`) option
    might cause deadlock failures if certain system catalogs are
    processed in parallel.

  - <span class="term">`-q`  
    </span><span class="term">`--quiet`</span>  
    Do not display progress messages.

  - <span class="term">`-t table` \[ (*`column`* \[,...\]) \]  
    </span><span class="term">`--table=table` \[ (*`column`* \[,...\])
    \]</span>  
    Clean or analyze *`table`* only. Column names can be specified only
    in conjunction with the `--analyze` or `--analyze-only` options.
    Multiple tables can be vacuumed by writing multiple `-t` switches.
    
    <div class="tip">
    
    ### Tip
    
    If you specify columns, you probably have to escape the parentheses
    from the shell. (See examples below.)
    
    </div>

  - <span class="term">`-v`  
    </span><span class="term">`--verbose`</span>  
    Print detailed information during processing.

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">vacuumdb</span> version and
    exit.

  - <span class="term">`-z`  
    </span><span class="term">`--analyze`</span>  
    Also calculate statistics for use by the optimizer.

  - <span class="term">`-Z`  
    </span><span class="term">`--analyze-only`</span>  
    Only calculate statistics for use by the optimizer (no vacuum).

  - <span class="term">`--analyze-in-stages`</span>  
    Only calculate statistics for use by the optimizer (no vacuum), like
    `--analyze-only`. Run several (currently three) stages of analyze
    with different configuration settings, to produce usable statistics
    faster.
    
    This option is useful to analyze a database that was newly populated
    from a restored dump or by `pg_upgrade`. This option will try to
    create some statistics as fast as possible, to make the database
    usable, and then produce full statistics in the subsequent stages.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">vacuumdb</span> command
    line arguments, and exit.

</div>

<span class="application">vacuumdb</span> also accepts the following
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
    Force <span class="application">vacuumdb</span> to prompt for a
    password before connecting to a database.
    
    This option is never essential, since
    <span class="application">vacuumdb</span> will automatically prompt
    for a password if the server demands password authentication.
    However, <span class="application">vacuumdb</span> will waste a
    connection attempt finding out that the server wants a password. In
    some cases it is worth typing `-W` to avoid the extra connection
    attempt.

  - <span class="term">`--maintenance-db=dbname`</span>  
    Specifies the name of the database to connect to discover what other
    databases should be vacuumed. If not specified, the `postgres`
    database will be used, and if that does not exist, `template1` will
    be used.

</div>

</div>

<div id="id-1.9.4.20.7" class="refsect1">

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

<div id="id-1.9.4.20.8" class="refsect1">

## Diagnostics

In case of difficulty, see
[<span class="refentrytitle">VACUUM</span>](sql-vacuum.html "VACUUM")
and
[<span class="refentrytitle"><span class="application">psql</span></span>](app-psql.html "psql")
for discussions of potential problems and error messages. The database
server must be running at the targeted host. Also, any default
connection settings and environment variables used by the
<span class="application">libpq</span> front-end library will apply.

</div>

<div id="id-1.9.4.20.9" class="refsect1">

## Notes

<span class="application">vacuumdb</span> might need to connect several
times to the <span class="productname">PostgreSQL</span> server, asking
for a password each time. It is convenient to have a `~/.pgpass` file in
such cases. See
[Section 33.15](libpq-pgpass.html "33.15. The Password File") for more
information.

</div>

<div id="id-1.9.4.20.10" class="refsect1">

## Examples

To clean the database `test`:

``` screen
$ vacuumdb test
```

To clean and analyze for the optimizer a database named `bigdb`:

``` screen
$ vacuumdb --analyze bigdb
```

To clean a single table `foo` in a database named `xyzzy`, and analyze a
single column `bar` of the table for the
optimizer:

``` screen
$ vacuumdb --analyze --verbose --table='foo(bar)' xyzzy
```

</div>

<div id="id-1.9.4.20.11" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle">VACUUM</span>](sql-vacuum.html "VACUUM")</span>

</div>

</div>

<div class="navfooter">

-----

|                                            |                             |                                |
| :----------------------------------------- | :-------------------------: | -----------------------------: |
| [Prev](app-reindexdb.html)                 | [Up](reference-client.html) |  [Next](reference-server.html) |
| <span class="application">reindexdb</span> |     [Home](index.html)      | PostgreSQL Server Applications |

</div>
