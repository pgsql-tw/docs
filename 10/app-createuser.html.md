<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">createuser</span>

</div>

[Prev](app-createdb.html "createdb") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-dropdb.html "dropdb")

-----

<div id="APP-CREATEUSER" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.5.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">createuser</span></span>

createuser — define a new <span class="productname">PostgreSQL</span>
user account

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`createuser` \[*`connection-option`*...\] \[*`option`*...\]
\[*`username`*\]

</div>

</div>

<div id="id-1.9.4.5.5" class="refsect1">

## Description

<span class="application">createuser</span> creates a new
<span class="productname">PostgreSQL</span> user (or more precisely, a
role). Only superusers and users with `CREATEROLE` privilege can create
new users, so <span class="application">createuser</span> must be
invoked by someone who can connect as a superuser or a user with
`CREATEROLE` privilege.

If you wish to create a new superuser, you must connect as a superuser,
not merely with `CREATEROLE` privilege. Being a superuser implies the
ability to bypass all access permission checks within the database, so
superuserdom should not be granted lightly.

<span class="application">createuser</span> is a wrapper around the SQL
command [<span class="refentrytitle">CREATE
ROLE</span>](sql-createrole.html "CREATE ROLE"). There is no effective
difference between creating users via this utility and via other methods
for accessing the server.

</div>

<div id="id-1.9.4.5.6" class="refsect1">

## Options

<span class="application">createuser</span> accepts the following
command-line arguments:

<div class="variablelist">

  - <span class="term">*`username`*</span>  
    Specifies the name of the
    <span class="productname">PostgreSQL</span> user to be created. This
    name must be different from all existing roles in this
    <span class="productname">PostgreSQL</span> installation.

  - <span class="term">`-c number`  
    </span><span class="term">`--connection-limit=number`</span>  
    Set a maximum number of connections for the new user. The default is
    to set no limit.

  - <span class="term">`-d`  
    </span><span class="term">`--createdb`</span>  
    The new user will be allowed to create databases.

  - <span class="term">`-D`  
    </span><span class="term">`--no-createdb`</span>  
    The new user will not be allowed to create databases. This is the
    default.

  - <span class="term">`-e`  
    </span><span class="term">`--echo`</span>  
    Echo the commands that <span class="application">createuser</span>
    generates and sends to the server.

  - <span class="term">`-E`  
    </span><span class="term">`--encrypted`</span>  
    This option is obsolete but still accepted for backward
    compatibility.

  - <span class="term">`-g role`  
    </span><span class="term">`--role=role`</span>  
    Indicates role to which this role will be added immediately as a new
    member. Multiple roles to which this role will be added as a member
    can be specified by writing multiple `-g` switches.

  - <span class="term">`-i`  
    </span><span class="term">`--inherit`</span>  
    The new role will automatically inherit privileges of roles it is a
    member of. This is the default.

  - <span class="term">`-I`  
    </span><span class="term">`--no-inherit`</span>  
    The new role will not automatically inherit privileges of roles it
    is a member of.

  - <span class="term">`--interactive`</span>  
    Prompt for the user name if none is specified on the command line,
    and also prompt for whichever of the options `-d`/`-D`, `-r`/`-R`,
    `-s`/`-S` is not specified on the command line. (This was the
    default behavior up to PostgreSQL 9.1.)

  - <span class="term">`-l`  
    </span><span class="term">`--login`</span>  
    The new user will be allowed to log in (that is, the user name can
    be used as the initial session user identifier). This is the
    default.

  - <span class="term">`-L`  
    </span><span class="term">`--no-login`</span>  
    The new user will not be allowed to log in. (A role without login
    privilege is still useful as a means of managing database
    permissions.)

  - <span class="term">`-P`  
    </span><span class="term">`--pwprompt`</span>  
    If given, <span class="application">createuser</span> will issue a
    prompt for the password of the new user. This is not necessary if
    you do not plan on using password authentication.

  - <span class="term">`-r`  
    </span><span class="term">`--createrole`</span>  
    The new user will be allowed to create new roles (that is, this user
    will have `CREATEROLE` privilege).

  - <span class="term">`-R`  
    </span><span class="term">`--no-createrole`</span>  
    The new user will not be allowed to create new roles. This is the
    default.

  - <span class="term">`-s`  
    </span><span class="term">`--superuser`</span>  
    The new user will be a superuser.

  - <span class="term">`-S`  
    </span><span class="term">`--no-superuser`</span>  
    The new user will not be a superuser. This is the default.

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">createuser</span> version and
    exit.

  - <span class="term">`--replication`</span>  
    The new user will have the `REPLICATION` privilege, which is
    described more fully in the documentation for
    [<span class="refentrytitle">CREATE
    ROLE</span>](sql-createrole.html "CREATE ROLE").

  - <span class="term">`--no-replication`</span>  
    The new user will not have the `REPLICATION` privilege, which is
    described more fully in the documentation for
    [<span class="refentrytitle">CREATE
    ROLE</span>](sql-createrole.html "CREATE ROLE").

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">createuser</span> command
    line arguments, and exit.

</div>

<span class="application">createuser</span> also accepts the following
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
    User name to connect as (not the user name to create).

  - <span class="term">`-w`  
    </span><span class="term">`--no-password`</span>  
    Never issue a password prompt. If the server requires password
    authentication and a password is not available by other means such
    as a `.pgpass` file, the connection attempt will fail. This option
    can be useful in batch jobs and scripts where no user is present to
    enter a password.

  - <span class="term">`-W`  
    </span><span class="term">`--password`</span>  
    Force <span class="application">createuser</span> to prompt for a
    password (for connecting to the server, not for the password of the
    new user).
    
    This option is never essential, since
    <span class="application">createuser</span> will automatically
    prompt for a password if the server demands password authentication.
    However, <span class="application">createuser</span> will waste a
    connection attempt finding out that the server wants a password. In
    some cases it is worth typing `-W` to avoid the extra connection
    attempt.

</div>

</div>

<div id="id-1.9.4.5.7" class="refsect1">

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

<div id="id-1.9.4.5.8" class="refsect1">

## Diagnostics

In case of difficulty, see [<span class="refentrytitle">CREATE
ROLE</span>](sql-createrole.html "CREATE ROLE") and
[<span class="refentrytitle"><span class="application">psql</span></span>](app-psql.html "psql")
for discussions of potential problems and error messages. The database
server must be running at the targeted host. Also, any default
connection settings and environment variables used by the
<span class="application">libpq</span> front-end library will apply.

</div>

<div id="id-1.9.4.5.9" class="refsect1">

## Examples

To create a user `joe` on the default database server:

``` screen
$ createuser joe
```

To create a user `joe` on the default database server with prompting for
some additional attributes:

``` screen
$ createuser --interactive joe
Shall the new role be a superuser? (y/n) n
Shall the new role be allowed to create databases? (y/n) n
Shall the new role be allowed to create more new roles? (y/n) n
```

To create the same user `joe` using the server on host `eden`, port
5000, with attributes explicitly specified, taking a look at the
underlying command:

``` screen
$ createuser -h eden -p 5000 -S -D -R -e joe
CREATE ROLE joe NOSUPERUSER NOCREATEDB NOCREATEROLE INHERIT LOGIN;
```

To create the user `joe` as a superuser, and assign a password
immediately:

``` screen
$ createuser -P -s -e joe
Enter password for new role: xyzzy
Enter it again: xyzzy
CREATE ROLE joe PASSWORD 'md5b5f5ba1a423792b526f799ae4eb3d59e' SUPERUSER CREATEDB CREATEROLE INHERIT LOGIN;
```

In the above example, the new password isn't actually echoed when typed,
but we show what was typed for clarity. As you see, the password is
encrypted before it is sent to the
client.

</div>

<div id="id-1.9.4.5.10" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle"><span class="application">dropuser</span></span>](app-dropuser.html "dropuser"),
[<span class="refentrytitle">CREATE
ROLE</span>](sql-createrole.html "CREATE ROLE")</span>

</div>

</div>

<div class="navfooter">

-----

|                                           |                             |                                         |
| :---------------------------------------- | :-------------------------: | --------------------------------------: |
| [Prev](app-createdb.html)                 | [Up](reference-client.html) |                 [Next](app-dropdb.html) |
| <span class="application">createdb</span> |     [Home](index.html)      | <span class="application">dropdb</span> |

</div>
