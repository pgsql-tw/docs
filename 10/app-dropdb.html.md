<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">dropdb</span>

</div>

[Prev](app-createuser.html "createuser") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-dropuser.html "dropuser")

-----

<div id="APP-DROPDB" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.6.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">dropdb</span></span>

dropdb — remove a <span class="productname">PostgreSQL</span> database

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`dropdb` \[*`connection-option`*...\] \[*`option`*...\] *`dbname`*

</div>

</div>

<div id="id-1.9.4.6.5" class="refsect1">

## Description

<span class="application">dropdb</span> destroys an existing
<span class="productname">PostgreSQL</span> database. The user who
executes this command must be a database superuser or the owner of the
database.

<span class="application">dropdb</span> is a wrapper around the SQL
command [<span class="refentrytitle">DROP
DATABASE</span>](sql-dropdatabase.html "DROP DATABASE"). There is no
effective difference between dropping databases via this utility and via
other methods for accessing the server.

</div>

<div id="id-1.9.4.6.6" class="refsect1">

## Options

<span class="application">dropdb</span> accepts the following
command-line arguments:

<div class="variablelist">

  - <span class="term">*`dbname`*</span>  
    Specifies the name of the database to be removed.

  - <span class="term">`-e`  
    </span><span class="term">`--echo`</span>  
    Echo the commands that <span class="application">dropdb</span>
    generates and sends to the server.

  - <span class="term">`-i`  
    </span><span class="term">`--interactive`</span>  
    Issues a verification prompt before doing anything destructive.

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">dropdb</span> version and exit.

  - <span class="term">`--if-exists`</span>  
    Do not throw an error if the database does not exist. A notice is
    issued in this case.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">dropdb</span> command line
    arguments, and exit.

</div>

<span class="application">dropdb</span> also accepts the following
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
    Force <span class="application">dropdb</span> to prompt for a
    password before connecting to a database.
    
    This option is never essential, since
    <span class="application">dropdb</span> will automatically prompt
    for a password if the server demands password authentication.
    However, <span class="application">dropdb</span> will waste a
    connection attempt finding out that the server wants a password. In
    some cases it is worth typing `-W` to avoid the extra connection
    attempt.

  - <span class="term">`--maintenance-db=dbname`</span>  
    Specifies the name of the database to connect to in order to drop
    the target database. If not specified, the `postgres` database will
    be used; if that does not exist (or is the database being dropped),
    `template1` will be used.

</div>

</div>

<div id="id-1.9.4.6.7" class="refsect1">

## Environment

<div class="variablelist">

  - <span class="term">`PGHOST`  
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

<div id="id-1.9.4.6.8" class="refsect1">

## Diagnostics

In case of difficulty, see [<span class="refentrytitle">DROP
DATABASE</span>](sql-dropdatabase.html "DROP DATABASE") and
[<span class="refentrytitle"><span class="application">psql</span></span>](app-psql.html "psql")
for discussions of potential problems and error messages. The database
server must be running at the targeted host. Also, any default
connection settings and environment variables used by the
<span class="application">libpq</span> front-end library will apply.

</div>

<div id="id-1.9.4.6.9" class="refsect1">

## Examples

To destroy the database `demo` on the default database server:

``` screen
$ dropdb demo
```

To destroy the database `demo` using the server on host `eden`, port
5000, with verification and a peek at the underlying command:

``` screen
$ dropdb -p 5000 -h eden -i -e demo
Database "demo" will be permanently deleted.
Are you sure? (y/n) y
DROP DATABASE demo;
```

</div>

<div id="id-1.9.4.6.10" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle"><span class="application">createdb</span></span>](app-createdb.html "createdb"),
[<span class="refentrytitle">DROP
DATABASE</span>](sql-dropdatabase.html "DROP DATABASE")</span>

</div>

</div>

<div class="navfooter">

-----

|                                             |                             |                                           |
| :------------------------------------------ | :-------------------------: | ----------------------------------------: |
| [Prev](app-createuser.html)                 | [Up](reference-client.html) |                 [Next](app-dropuser.html) |
| <span class="application">createuser</span> |     [Home](index.html)      | <span class="application">dropuser</span> |

</div>
