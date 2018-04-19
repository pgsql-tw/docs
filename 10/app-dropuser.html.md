<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">dropuser</span>

</div>

[Prev](app-dropdb.html "dropdb") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-ecpg.html "ecpg")

-----

<div id="APP-DROPUSER" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.7.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">dropuser</span></span>

dropuser — remove a <span class="productname">PostgreSQL</span> user
account

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`dropuser` \[*`connection-option`*...\] \[*`option`*...\]
\[*`username`*\]

</div>

</div>

<div id="id-1.9.4.7.5" class="refsect1">

## Description

<span class="application">dropuser</span> removes an existing
<span class="productname">PostgreSQL</span> user. Only superusers and
users with the `CREATEROLE` privilege can remove
<span class="productname">PostgreSQL</span> users. (To remove a
superuser, you must yourself be a superuser.)

<span class="application">dropuser</span> is a wrapper around the SQL
command [<span class="refentrytitle">DROP
ROLE</span>](sql-droprole.html "DROP ROLE"). There is no effective
difference between dropping users via this utility and via other methods
for accessing the server.

</div>

<div id="id-1.9.4.7.6" class="refsect1">

## Options

<span class="application">dropuser</span> accepts the following
command-line arguments:

<div class="variablelist">

  - <span class="term">*`username`*</span>  
    Specifies the name of the
    <span class="productname">PostgreSQL</span> user to be removed. You
    will be prompted for a name if none is specified on the command line
    and the `-i`/`--interactive` option is used.

  - <span class="term">`-e`  
    </span><span class="term">`--echo`</span>  
    Echo the commands that <span class="application">dropuser</span>
    generates and sends to the server.

  - <span class="term">`-i`  
    </span><span class="term">`--interactive`</span>  
    Prompt for confirmation before actually removing the user, and
    prompt for the user name if none is specified on the command line.

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">dropuser</span> version and
    exit.

  - <span class="term">`--if-exists`</span>  
    Do not throw an error if the user does not exist. A notice is issued
    in this case.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">dropuser</span> command
    line arguments, and exit.

</div>

<span class="application">dropuser</span> also accepts the following
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
    User name to connect as (not the user name to drop).

  - <span class="term">`-w`  
    </span><span class="term">`--no-password`</span>  
    Never issue a password prompt. If the server requires password
    authentication and a password is not available by other means such
    as a `.pgpass` file, the connection attempt will fail. This option
    can be useful in batch jobs and scripts where no user is present to
    enter a password.

  - <span class="term">`-W`  
    </span><span class="term">`--password`</span>  
    Force <span class="application">dropuser</span> to prompt for a
    password before connecting to a database.
    
    This option is never essential, since
    <span class="application">dropuser</span> will automatically prompt
    for a password if the server demands password authentication.
    However, <span class="application">dropuser</span> will waste a
    connection attempt finding out that the server wants a password. In
    some cases it is worth typing `-W` to avoid the extra connection
    attempt.

</div>

</div>

<div id="id-1.9.4.7.7" class="refsect1">

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

<div id="id-1.9.4.7.8" class="refsect1">

## Diagnostics

In case of difficulty, see [<span class="refentrytitle">DROP
ROLE</span>](sql-droprole.html "DROP ROLE") and
[<span class="refentrytitle"><span class="application">psql</span></span>](app-psql.html "psql")
for discussions of potential problems and error messages. The database
server must be running at the targeted host. Also, any default
connection settings and environment variables used by the
<span class="application">libpq</span> front-end library will apply.

</div>

<div id="id-1.9.4.7.9" class="refsect1">

## Examples

To remove user `joe` from the default database server:

``` screen
$ dropuser joe
```

To remove user `joe` using the server on host `eden`, port 5000, with
verification and a peek at the underlying command:

``` screen
$ dropuser -p 5000 -h eden -i -e joe
Role "joe" will be permanently removed.
Are you sure? (y/n) y
DROP ROLE joe;
```

</div>

<div id="id-1.9.4.7.10" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle"><span class="application">createuser</span></span>](app-createuser.html "createuser"),
[<span class="refentrytitle">DROP
ROLE</span>](sql-droprole.html "DROP ROLE")</span>

</div>

</div>

<div class="navfooter">

-----

|                                         |                             |                                       |
| :-------------------------------------- | :-------------------------: | ------------------------------------: |
| [Prev](app-dropdb.html)                 | [Up](reference-client.html) |                 [Next](app-ecpg.html) |
| <span class="application">dropdb</span> |     [Home](index.html)      | <span class="application">ecpg</span> |

</div>
