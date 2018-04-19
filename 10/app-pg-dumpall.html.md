<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">pg\_dumpall</span>

</div>

[Prev](app-pgdump.html "pg_dump") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-pg-isready.html "pg_isready")

-----

<div id="APP-PG-DUMPALL" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.13.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">pg\_dumpall</span></span>

pg\_dumpall — extract a <span class="productname">PostgreSQL</span>
database cluster into a script file

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`pg_dumpall` \[*`connection-option`*...\] \[*`option`*...\]

</div>

</div>

<div id="APP-PG-DUMPALL-DESCRIPTION" class="refsect1">

## Description

<span class="application">pg\_dumpall</span> is a utility for writing
out (<span class="quote">“<span class="quote">dumping</span>”</span>)
all <span class="productname">PostgreSQL</span> databases of a cluster
into one script file. The script file contains SQL commands that can be
used as input to
[<span class="refentrytitle"><span class="application">psql</span></span>](app-psql.html "psql")
to restore the databases. It does this by calling
[<span class="refentrytitle">pg\_dump</span>](app-pgdump.html "pg_dump")
for each database in a cluster.
<span class="application">pg\_dumpall</span> also dumps global objects
that are common to all databases.
(<span class="application">pg\_dump</span> does not save these objects.)
This currently includes information about database users and groups,
tablespaces, and properties such as access permissions that apply to
databases as a whole.

Since <span class="application">pg\_dumpall</span> reads tables from all
databases you will most likely have to connect as a database superuser
in order to produce a complete dump. Also you will need superuser
privileges to execute the saved script in order to be allowed to add
users and groups, and to create databases.

The SQL script will be written to the standard output. Use the
\[-f|file\] option or shell operators to redirect it into a file.

<span class="application">pg\_dumpall</span> needs to connect several
times to the <span class="productname">PostgreSQL</span> server (once
per database). If you use password authentication it will ask for a
password each time. It is convenient to have a `~/.pgpass` file in such
cases. See [Section 33.15](libpq-pgpass.html "33.15. The Password File")
for more information.

</div>

<div id="id-1.9.4.13.6" class="refsect1">

## Options

The following command-line options control the content and format of the
output.

<div class="variablelist">

  - <span class="term">`-a`  
    </span><span class="term">`--data-only`</span>  
    Dump only the data, not the schema (data definitions).

  - <span class="term">`-c`  
    </span><span class="term">`--clean`</span>  
    Include SQL commands to clean (drop) databases before recreating
    them. `DROP` commands for roles and tablespaces are added as well.

  - <span class="term">`-f filename`  
    </span><span class="term">`--file=filename`</span>  
    Send output to the specified file. If this is omitted, the standard
    output is used.

  - <span class="term">`-g`  
    </span><span class="term">`--globals-only`</span>  
    Dump only global objects (roles and tablespaces), no databases.

  - <span class="term">`-o`  
    </span><span class="term">`--oids`</span>  
    Dump object identifiers (OIDs) as part of the data for every table.
    Use this option if your application references the OID columns in
    some way (e.g., in a foreign key constraint). Otherwise, this option
    should not be used.

  - <span class="term">`-O`  
    </span><span class="term">`--no-owner`</span>  
    Do not output commands to set ownership of objects to match the
    original database. By default,
    <span class="application">pg\_dumpall</span> issues `ALTER OWNER` or
    `SET SESSION AUTHORIZATION` statements to set ownership of created
    schema elements. These statements will fail when the script is run
    unless it is started by a superuser (or the same user that owns all
    of the objects in the script). To make a script that can be restored
    by any user, but will give that user ownership of all the objects,
    specify `-O`.

  - <span class="term">`-r`  
    </span><span class="term">`--roles-only`</span>  
    Dump only roles, no databases or tablespaces.

  - <span class="term">`-s`  
    </span><span class="term">`--schema-only`</span>  
    Dump only the object definitions (schema), not data.

  - <span class="term">`-S username`  
    </span><span class="term">`--superuser=username`</span>  
    Specify the superuser user name to use when disabling triggers. This
    is relevant only if `--disable-triggers` is used. (Usually, it's
    better to leave this out, and instead start the resulting script as
    superuser.)

  - <span class="term">`-t`  
    </span><span class="term">`--tablespaces-only`</span>  
    Dump only tablespaces, no databases or roles.

  - <span class="term">`-v`  
    </span><span class="term">`--verbose`</span>  
    Specifies verbose mode. This will cause
    <span class="application">pg\_dumpall</span> to output start/stop
    times to the dump file, and progress messages to standard error. It
    will also enable verbose output in
    <span class="application">pg\_dump</span>.

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">pg\_dumpall</span> version and
    exit.

  - <span class="term">`-x`  
    </span><span class="term">`--no-privileges`  
    </span><span class="term">`--no-acl`</span>  
    Prevent dumping of access privileges (grant/revoke commands).

  - <span class="term">`--binary-upgrade`</span>  
    This option is for use by in-place upgrade utilities. Its use for
    other purposes is not recommended or supported. The behavior of the
    option may change in future releases without notice.

  - <span class="term">`--column-inserts`  
    </span><span class="term">`--attribute-inserts`</span>  
    Dump data as `INSERT` commands with explicit column names (`INSERT
    INTO table` (*`column`*, ...) VALUES ...). This will make
    restoration very slow; it is mainly useful for making dumps that can
    be loaded into non-<span class="productname">PostgreSQL</span>
    databases.

  - <span class="term">`--disable-dollar-quoting`</span>  
    This option disables the use of dollar quoting for function bodies,
    and forces them to be quoted using SQL standard string syntax.

  - <span class="term">`--disable-triggers`</span>  
    This option is relevant only when creating a data-only dump. It
    instructs <span class="application">pg\_dumpall</span> to include
    commands to temporarily disable triggers on the target tables while
    the data is reloaded. Use this if you have referential integrity
    checks or other triggers on the tables that you do not want to
    invoke during data reload.
    
    Presently, the commands emitted for `--disable-triggers` must be
    done as superuser. So, you should also specify a superuser name with
    `-S`, or preferably be careful to start the resulting script as a
    superuser.

  - <span class="term">`--if-exists`</span>  
    Use conditional commands (i.e. add an `IF EXISTS` clause) to clean
    databases and other objects. This option is not valid unless
    `--clean` is also specified.

  - <span class="term">`--inserts`</span>  
    Dump data as `INSERT` commands (rather than `COPY`). This will make
    restoration very slow; it is mainly useful for making dumps that can
    be loaded into non-<span class="productname">PostgreSQL</span>
    databases. Note that the restore might fail altogether if you have
    rearranged column order. The `--column-inserts` option is safer,
    though even slower.

  - <span class="term">`--lock-wait-timeout=timeout`</span>  
    Do not wait forever to acquire shared table locks at the beginning
    of the dump. Instead, fail if unable to lock a table within the
    specified *`timeout`*. The timeout may be specified in any of the
    formats accepted by `SET statement_timeout`. Allowed values vary
    depending on the server version you are dumping from, but an integer
    number of milliseconds is accepted by all versions since 7.3. This
    option is ignored when dumping from a pre-7.3 server.

  - <span class="term">`--no-publications`</span>  
    Do not dump publications.

  - <span class="term">`--no-role-passwords`</span>  
    Do not dump passwords for roles. When restored, roles will have a
    null password, and password authentication will always fail until
    the password is set. Since password values aren't needed when this
    option is specified, the role information is read from the catalog
    view `pg_roles` instead of `pg_authid`. Therefore, this option also
    helps if access to `pg_authid` is restricted by some security
    policy.

  - <span class="term">`--no-security-labels`</span>  
    Do not dump security labels.

  - <span class="term">`--no-subscriptions`</span>  
    Do not dump subscriptions.

  - <span class="term">`--no-sync`</span>  
    By default, `pg_dumpall` will wait for all files to be written
    safely to disk. This option causes `pg_dumpall` to return without
    waiting, which is faster, but means that a subsequent operating
    system crash can leave the dump corrupt. Generally, this option is
    useful for testing but should not be used when dumping data from
    production installation.

  - <span class="term">`--no-tablespaces`</span>  
    Do not output commands to create tablespaces nor select tablespaces
    for objects. With this option, all objects will be created in
    whichever tablespace is the default during restore.

  - <span class="term">`--no-unlogged-table-data`</span>  
    Do not dump the contents of unlogged tables. This option has no
    effect on whether or not the table definitions (schema) are dumped;
    it only suppresses dumping the table data.

  - <span class="term">`--quote-all-identifiers`</span>  
    Force quoting of all identifiers. This option is recommended when
    dumping a database from a server whose
    <span class="productname">PostgreSQL</span> major version is
    different from <span class="application">pg\_dumpall</span>'s, or
    when the output is intended to be loaded into a server of a
    different major version. By default,
    <span class="application">pg\_dumpall</span> quotes only identifiers
    that are reserved words in its own major version. This sometimes
    results in compatibility issues when dealing with servers of other
    versions that may have slightly different sets of reserved words.
    Using `--quote-all-identifiers` prevents such issues, at the price
    of a harder-to-read dump script.

  - <span class="term">`--use-set-session-authorization`</span>  
    Output SQL-standard `SET SESSION AUTHORIZATION` commands instead of
    `ALTER OWNER` commands to determine object ownership. This makes the
    dump more standards compatible, but depending on the history of the
    objects in the dump, might not restore properly.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">pg\_dumpall</span> command
    line arguments, and exit.

</div>

The following command-line options control the database connection
parameters.

<div class="variablelist">

  - <span class="term">`-d connstr`  
    </span><span class="term">`--dbname=connstr`</span>  
    Specifies parameters used to connect to the server, as a connection
    string. See
    [Section 33.1.1](libpq-connect.html#LIBPQ-CONNSTRING "33.1.1. Connection Strings")
    for more information.
    
    The option is called `--dbname` for consistency with other client
    applications, but because
    <span class="application">pg\_dumpall</span> needs to connect to
    many databases, database name in the connection string will be
    ignored. Use `-l` option to specify the name of the database used to
    dump global objects and to discover what other databases should be
    dumped.

  - <span class="term">`-h host`  
    </span><span class="term">`--host=host`</span>  
    Specifies the host name of the machine on which the database server
    is running. If the value begins with a slash, it is used as the
    directory for the Unix domain socket. The default is taken from the
    `PGHOST` environment variable, if set, else a Unix domain socket
    connection is attempted.

  - <span class="term">`-l dbname`  
    </span><span class="term">`--database=dbname`</span>  
    Specifies the name of the database to connect to for dumping global
    objects and discovering what other databases should be dumped. If
    not specified, the `postgres` database will be used, and if that
    does not exist, `template1` will be used.

  - <span class="term">`-p port`  
    </span><span class="term">`--port=port`</span>  
    Specifies the TCP port or local Unix domain socket file extension on
    which the server is listening for connections. Defaults to the
    `PGPORT` environment variable, if set, or a compiled-in default.

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
    Force <span class="application">pg\_dumpall</span> to prompt for a
    password before connecting to a database.
    
    This option is never essential, since
    <span class="application">pg\_dumpall</span> will automatically
    prompt for a password if the server demands password authentication.
    However, <span class="application">pg\_dumpall</span> will waste a
    connection attempt finding out that the server wants a password. In
    some cases it is worth typing `-W` to avoid the extra connection
    attempt.
    
    Note that the password prompt will occur again for each database to
    be dumped. Usually, it's better to set up a `~/.pgpass` file than to
    rely on manual password entry.

  - <span class="term">`--role=rolename`</span>  
    Specifies a role name to be used to create the dump. This option
    causes <span class="application">pg\_dumpall</span> to issue a `SET
    ROLE` *`rolename`* command after connecting to the database. It is
    useful when the authenticated user (specified by `-U`) lacks
    privileges needed by <span class="application">pg\_dumpall</span>,
    but can switch to a role with the required rights. Some
    installations have a policy against logging in directly as a
    superuser, and use of this option allows dumps to be made without
    violating the policy.

</div>

</div>

<div id="id-1.9.4.13.7" class="refsect1">

## Environment

<div class="variablelist">

  - <span class="term">`PGHOST`  
    </span><span class="term">`PGOPTIONS`  
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

<div id="id-1.9.4.13.8" class="refsect1">

## Notes

Since <span class="application">pg\_dumpall</span> calls
<span class="application">pg\_dump</span> internally, some diagnostic
messages will refer to <span class="application">pg\_dump</span>.

Once restored, it is wise to run `ANALYZE` on each database so the
optimizer has useful statistics. You can also run `vacuumdb -a -z` to
analyze all databases.

<span class="application">pg\_dumpall</span> requires all needed
tablespace directories to exist before the restore; otherwise, database
creation will fail for databases in non-default locations.

</div>

<div id="APP-PG-DUMPALL-EX" class="refsect1">

## Examples

To dump all databases:

``` screen
$ pg_dumpall > db.out
```

To reload database(s) from this file, you can use:

``` screen
$ psql -f db.out postgres
```

(It is not important to which database you connect here since the script
file created by <span class="application">pg\_dumpall</span> will
contain the appropriate commands to create and connect to the saved
databases.)

</div>

<div id="id-1.9.4.13.10" class="refsect1">

## See Also

Check
[<span class="refentrytitle">pg\_dump</span>](app-pgdump.html "pg_dump")
for details on possible error
conditions.

</div>

</div>

<div class="navfooter">

-----

|                         |                             |                                              |
| :---------------------- | :-------------------------: | -------------------------------------------: |
| [Prev](app-pgdump.html) | [Up](reference-client.html) |                  [Next](app-pg-isready.html) |
| pg\_dump                |     [Home](index.html)      | <span class="application">pg\_isready</span> |

</div>
