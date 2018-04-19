<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">pg\_isready</span>

</div>

[Prev](app-pg-dumpall.html "pg_dumpall") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-pgreceivewal.html "pg_receivewal")

-----

<div id="APP-PG-ISREADY" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.14.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">pg\_isready</span></span>

pg\_isready — check the connection status of a
<span class="productname">PostgreSQL</span> server

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`pg_isready` \[*`connection-option`*...\] \[*`option`*...\]

</div>

</div>

<div id="APP-PG-ISREADY-DESCRIPTION" class="refsect1">

## Description

<span class="application">pg\_isready</span> is a utility for checking
the connection status of a <span class="productname">PostgreSQL</span>
database server. The exit status specifies the result of the connection
check.

</div>

<div id="APP-PG-ISREADY-OPTIONS" class="refsect1">

## Options

<div class="variablelist">

  - <span class="term">`-d dbname`  
    </span><span class="term">`--dbname=dbname`</span>  
    Specifies the name of the database to connect to.
    
    If this parameter contains an `=` sign or starts with a valid URI
    prefix (`postgresql://` or `postgres://`), it is treated as a
    *`conninfo`* string. See
    [Section 33.1.1](libpq-connect.html#LIBPQ-CONNSTRING "33.1.1. Connection Strings")
    for more information.

  - <span class="term">`-h hostname`  
    </span><span class="term">`--host=hostname`</span>  
    Specifies the host name of the machine on which the server is
    running. If the value begins with a slash, it is used as the
    directory for the Unix-domain socket.

  - <span class="term">`-p port`  
    </span><span class="term">`--port=port`</span>  
    Specifies the TCP port or the local Unix-domain socket file
    extension on which the server is listening for connections. Defaults
    to the value of the `PGPORT` environment variable or, if not set, to
    the port specified at compile time, usually 5432.

  - <span class="term">`-q`  
    </span><span class="term">`--quiet`</span>  
    Do not display status message. This is useful when scripting.

  - <span class="term">`-t seconds`  
    </span><span class="term">`--timeout=seconds`</span>  
    The maximum number of seconds to wait when attempting connection
    before returning that the server is not responding. Setting to 0
    disables. The default is 3 seconds.

  - <span class="term">`-U username`  
    </span><span class="term">`--username=username`</span>  
    Connect to the database as the user *`username`* instead of the
    default.

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">pg\_isready</span> version and
    exit.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">pg\_isready</span> command
    line arguments, and exit.

</div>

</div>

<div id="id-1.9.4.14.7" class="refsect1">

## Exit Status

<span class="application">pg\_isready</span> returns `0` to the shell if
the server is accepting connections normally, `1` if the server is
rejecting connections (for example during startup), `2` if there was no
response to the connection attempt, and `3` if no attempt was made (for
example due to invalid parameters).

</div>

<div id="id-1.9.4.14.8" class="refsect1">

## Environment

`pg_isready`, like most other
<span class="productname">PostgreSQL</span> utilities, also uses the
environment variables supported by
<span class="application">libpq</span> (see
[Section 33.14](libpq-envars.html "33.14. Environment Variables")).

</div>

<div id="APP-PG-ISREADY-NOTES" class="refsect1">

## Notes

It is not necessary to supply correct user name, password, or database
name values to obtain the server status; however, if incorrect values
are provided, the server will log a failed connection attempt.

</div>

<div id="APP-PG-ISREADY-EXAMPLES" class="refsect1">

## Examples

Standard Usage:

``` screen
$ pg_isready
/tmp:5432 - accepting connections
$ echo $?
0
```

Running with connection parameters to a
<span class="productname">PostgreSQL</span> cluster in startup:

``` screen
$ pg_isready -h localhost -p 5433
localhost:5433 - rejecting connections
$ echo $?
1
```

Running with connection parameters to a non-responsive
<span class="productname">PostgreSQL</span> cluster:

``` screen
$ pg_isready -h someremotehost
someremotehost:5432 - no response
$ echo $?
2
```

</div>

</div>

<div class="navfooter">

-----

|                                              |                             |                               |
| :------------------------------------------- | :-------------------------: | ----------------------------: |
| [Prev](app-pg-dumpall.html)                  | [Up](reference-client.html) | [Next](app-pgreceivewal.html) |
| <span class="application">pg\_dumpall</span> |     [Home](index.html)      |                pg\_receivewal |

</div>
