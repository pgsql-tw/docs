<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">createdb</span>

</div>

[Prev](app-clusterdb.html "clusterdb") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-createuser.html "createuser")

-----

<div id="APP-CREATEDB" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.4.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">createdb</span></span>

createdb — create a new <span class="productname">PostgreSQL</span>
database

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`createdb` \[*`connection-option`*...\] \[*`option`*...\] \[*`dbname`*
\[*`description`*\]\]

</div>

</div>

<div id="R1-APP-CREATEDB-1" class="refsect1">

## Description

<span class="application">createdb</span> creates a new
<span class="productname">PostgreSQL</span> database.

Normally, the database user who executes this command becomes the owner
of the new database. However, a different owner can be specified via the
`-O` option, if the executing user has appropriate privileges.

<span class="application">createdb</span> is a wrapper around the SQL
command [<span class="refentrytitle">CREATE
DATABASE</span>](sql-createdatabase.html "CREATE DATABASE"). There is no
effective difference between creating databases via this utility and via
other methods for accessing the server.

</div>

<div id="id-1.9.4.4.6" class="refsect1">

## Options

<span class="application">createdb</span> accepts the following
command-line arguments:

<div class="variablelist">

  - <span class="term">*`dbname`*</span>  
    Specifies the name of the database to be created. The name must be
    unique among all <span class="productname">PostgreSQL</span>
    databases in this cluster. The default is to create a database with
    the same name as the current system user.

  - <span class="term">*`description`*</span>  
    Specifies a comment to be associated with the newly created
    database.

  - <span class="term">`-D tablespace`  
    </span><span class="term">`--tablespace=tablespace`</span>  
    Specifies the default tablespace for the database. (This name is
    processed as a double-quoted identifier.)

  - <span class="term">`-e`  
    </span><span class="term">`--echo`</span>  
    Echo the commands that <span class="application">createdb</span>
    generates and sends to the server.

  - <span class="term">`-E encoding`  
    </span><span class="term">`--encoding=encoding`</span>  
    Specifies the character encoding scheme to be used in this database.
    The character sets supported by the
    <span class="productname">PostgreSQL</span> server are described in
    [Section 23.3.1](multibyte.html#MULTIBYTE-CHARSET-SUPPORTED "23.3.1. Supported Character Sets").

  - <span class="term">`-l locale`  
    </span><span class="term">`--locale=locale`</span>  
    Specifies the locale to be used in this database. This is equivalent
    to specifying both `--lc-collate` and `--lc-ctype`.

  - <span class="term">`--lc-collate=locale`</span>  
    Specifies the LC\_COLLATE setting to be used in this database.

  - <span class="term">`--lc-ctype=locale`</span>  
    Specifies the LC\_CTYPE setting to be used in this database.

  - <span class="term">`-O owner`  
    </span><span class="term">`--owner=owner`</span>  
    Specifies the database user who will own the new database. (This
    name is processed as a double-quoted identifier.)

  - <span class="term">`-T template`  
    </span><span class="term">`--template=template`</span>  
    Specifies the template database from which to build this database.
    (This name is processed as a double-quoted identifier.)

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">createdb</span> version and
    exit.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">createdb</span> command
    line arguments, and exit.

</div>

The options `-D`, `-l`, `-E`, `-O`, and `-T` correspond to options of
the underlying SQL command [<span class="refentrytitle">CREATE
DATABASE</span>](sql-createdatabase.html "CREATE DATABASE"); see there
for more information about them.

<span class="application">createdb</span> also accepts the following
command-line arguments for connection parameters:

<div class="variablelist">

  - <span class="term">`-h host`  
    </span><span class="term">`--host=host`</span>  
    Specifies the host name of the machine on which the server is
    running. If the value begins with a slash, it is used as the
    directory for the Unix domain socket.

  - <span class="term">`-p port`  
    </span><span class="term">`--port=port`</span>  
    Specifies the TCP port or the local Unix domain socket file
    extension on which the server is listening for connections.

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
    Force <span class="application">createdb</span> to prompt for a
    password before connecting to a database.
    
    This option is never essential, since
    <span class="application">createdb</span> will automatically prompt
    for a password if the server demands password authentication.
    However, <span class="application">createdb</span> will waste a
    connection attempt finding out that the server wants a password. In
    some cases it is worth typing `-W` to avoid the extra connection
    attempt.

  - <span class="term">`--maintenance-db=dbname`</span>  
    Specifies the name of the database to connect to when creating the
    new database. If not specified, the `postgres` database will be
    used; if that does not exist (or if it is the name of the new
    database being created), `template1` will be used.

</div>

</div>

<div id="id-1.9.4.4.7" class="refsect1">

## Environment

<div class="variablelist">

  - <span class="term">`PGDATABASE`</span>  
    If set, the name of the database to create, unless overridden on the
    command line.

  - <span class="term">`PGHOST`  
    </span><span class="term">`PGPORT`  
    </span><span class="term">`PGUSER`</span>  
    Default connection parameters. `PGUSER` also determines the name of
    the database to create, if it is not specified on the command line
    or by `PGDATABASE`.

</div>

This utility, like most other
<span class="productname">PostgreSQL</span> utilities, also uses the
environment variables supported by
<span class="application">libpq</span> (see
[Section 33.14](libpq-envars.html "33.14. Environment Variables")).

</div>

<div id="id-1.9.4.4.8" class="refsect1">

## Diagnostics

In case of difficulty, see [<span class="refentrytitle">CREATE
DATABASE</span>](sql-createdatabase.html "CREATE DATABASE") and
[<span class="refentrytitle"><span class="application">psql</span></span>](app-psql.html "psql")
for discussions of potential problems and error messages. The database
server must be running at the targeted host. Also, any default
connection settings and environment variables used by the
<span class="application">libpq</span> front-end library will apply.

</div>

<div id="id-1.9.4.4.9" class="refsect1">

## Examples

To create the database `demo` using the default database server:

``` screen
$ createdb demo
```

To create the database `demo` using the server on host `eden`, port
5000, using the `template0` template database, here is the command-line
command and the underlying SQL command:

``` screen
$ createdb -p 5000 -h eden -T template0 -e demo
CREATE DATABASE demo TEMPLATE template0;
```

</div>

<div id="id-1.9.4.4.10" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle"><span class="application">dropdb</span></span>](app-dropdb.html "dropdb"),
[<span class="refentrytitle">CREATE
DATABASE</span>](sql-createdatabase.html "CREATE DATABASE")</span>

</div>

</div>

<div class="navfooter">

-----

|                                            |                             |                                             |
| :----------------------------------------- | :-------------------------: | ------------------------------------------: |
| [Prev](app-clusterdb.html)                 | [Up](reference-client.html) |                 [Next](app-createuser.html) |
| <span class="application">clusterdb</span> |     [Home](index.html)      | <span class="application">createuser</span> |

</div>
