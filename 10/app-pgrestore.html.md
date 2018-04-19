<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

pg\_restore

</div>

[Prev](app-pgrecvlogical.html "pg_recvlogical") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-psql.html "psql")

-----

<div id="APP-PGRESTORE" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.17.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle">pg\_restore</span>

pg\_restore — restore a <span class="productname">PostgreSQL</span>
database from an archive file created by
<span class="application">pg\_dump</span>

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`pg_restore` \[*`connection-option`*...\] \[*`option`*...\]
\[*`filename`*\]

</div>

</div>

<div id="APP-PGRESTORE-DESCRIPTION" class="refsect1">

## Description

<span class="application">pg\_restore</span> is a utility for restoring
a <span class="productname">PostgreSQL</span> database from an archive
created by
[<span class="refentrytitle">pg\_dump</span>](app-pgdump.html "pg_dump")
in one of the non-plain-text formats. It will issue the commands
necessary to reconstruct the database to the state it was in at the time
it was saved. The archive files also allow
<span class="application">pg\_restore</span> to be selective about what
is restored, or even to reorder the items prior to being restored. The
archive files are designed to be portable across architectures.

<span class="application">pg\_restore</span> can operate in two modes.
If a database name is specified,
<span class="application">pg\_restore</span> connects to that database
and restores archive contents directly into the database. Otherwise, a
script containing the SQL commands necessary to rebuild the database is
created and written to a file or standard output. This script output is
equivalent to the plain text output format of
<span class="application">pg\_dump</span>. Some of the options
controlling the output are therefore analogous to
<span class="application">pg\_dump</span> options.

Obviously, <span class="application">pg\_restore</span> cannot restore
information that is not present in the archive file. For instance, if
the archive was made using the
<span class="quote">“<span class="quote">dump data as `INSERT`
commands</span>”</span> option,
<span class="application">pg\_restore</span> will not be able to load
the data using `COPY` statements.

</div>

<div id="APP-PGRESTORE-OPTIONS" class="refsect1">

## Options

<span class="application">pg\_restore</span> accepts the following
command line arguments.

<div class="variablelist">

  - <span class="term">*`filename`*</span>  
    Specifies the location of the archive file (or directory, for a
    directory-format archive) to be restored. If not specified, the
    standard input is used.

  - <span class="term">`-a`  
    </span><span class="term">`--data-only`</span>  
    Restore only the data, not the schema (data definitions). Table
    data, large objects, and sequence values are restored, if present in
    the archive.
    
    This option is similar to, but for historical reasons not identical
    to, specifying `--section=data`.

  - <span class="term">`-c`  
    </span><span class="term">`--clean`</span>  
    Clean (drop) database objects before recreating them. (Unless
    `--if-exists` is used, this might generate some harmless error
    messages, if any objects were not present in the destination
    database.)

  - <span class="term">`-C`  
    </span><span class="term">`--create`</span>  
    Create the database before restoring into it. If `--clean` is also
    specified, drop and recreate the target database before connecting
    to it.
    
    When this option is used, the database named with `-d` is used only
    to issue the initial `DROP DATABASE` and `CREATE DATABASE` commands.
    All data is restored into the database name that appears in the
    archive.

  - <span class="term">`-d dbname`  
    </span><span class="term">`--dbname=dbname`</span>  
    Connect to database *`dbname`* and restore directly into the
    database.

  - <span class="term">`-e`  
    </span><span class="term">`--exit-on-error`</span>  
    Exit if an error is encountered while sending SQL commands to the
    database. The default is to continue and to display a count of
    errors at the end of the restoration.

  - <span class="term">`-f filename`  
    </span><span class="term">`--file=filename`</span>  
    Specify output file for generated script, or for the listing when
    used with `-l`. Default is the standard output.

  - <span class="term">`-F format`  
    </span><span class="term">`--format=format`</span>  
    Specify format of the archive. It is not necessary to specify the
    format, since <span class="application">pg\_restore</span> will
    determine the format automatically. If specified, it can be one of
    the following:
    
    <div class="variablelist">
    
      - <span class="term">`c`  
        </span><span class="term">`custom`</span>  
        The archive is in the custom format of
        <span class="application">pg\_dump</span>.
    
      - <span class="term">`d`  
        </span><span class="term">`directory`</span>  
        The archive is a directory archive.
    
      - <span class="term">`t`  
        </span><span class="term">`tar`</span>  
        The archive is a `tar` archive.
    
    </div>

  - <span class="term">`-I index`  
    </span><span class="term">`--index=index`</span>  
    Restore definition of named index only. Multiple indexes may be
    specified with multiple `-I` switches.

  - <span class="term">`-j number-of-jobs`  
    </span><span class="term">`--jobs=number-of-jobs`</span>  
    Run the most time-consuming parts of
    <span class="application">pg\_restore</span> — those which load
    data, create indexes, or create constraints — using multiple
    concurrent jobs. This option can dramatically reduce the time to
    restore a large database to a server running on a multiprocessor
    machine.
    
    Each job is one process or one thread, depending on the operating
    system, and uses a separate connection to the server.
    
    The optimal value for this option depends on the hardware setup of
    the server, of the client, and of the network. Factors include the
    number of CPU cores and the disk setup. A good place to start is the
    number of CPU cores on the server, but values larger than that can
    also lead to faster restore times in many cases. Of course, values
    that are too high will lead to decreased performance because of
    thrashing.
    
    Only the custom and directory archive formats are supported with
    this option. The input must be a regular file or directory (not, for
    example, a pipe). This option is ignored when emitting a script
    rather than connecting directly to a database server. Also, multiple
    jobs cannot be used together with the option `--single-transaction`.

  - <span class="term">`-l`  
    </span><span class="term">`--list`</span>  
    List the table of contents of the archive. The output of this
    operation can be used as input to the `-L` option. Note that if
    filtering switches such as `-n` or `-t` are used with `-l`, they
    will restrict the items listed.

  - <span class="term">`-L list-file`  
    </span><span class="term">`--use-list=list-file`</span>  
    Restore only those archive elements that are listed in
    *`list-file`*, and restore them in the order they appear in the
    file. Note that if filtering switches such as `-n` or `-t` are used
    with `-L`, they will further restrict the items restored.
    
    *`list-file`* is normally created by editing the output of a
    previous `-l` operation. Lines can be moved or removed, and can also
    be commented out by placing a semicolon (`;`) at the start of the
    line. See below for examples.

  - <span class="term">`-n schema`  
    </span><span class="term">`--schema=schema`</span>  
    Restore only objects that are in the named schema. Multiple schemas
    may be specified with multiple `-n` switches. This can be combined
    with the `-t` option to restore just a specific table.

  - <span class="term">`-N schema`  
    </span><span class="term">`--exclude-schema=schema`</span>  
    Do not restore objects that are in the named schema. Multiple
    schemas to be excluded may be specified with multiple `-N` switches.
    
    When both `-n` and `-N` are given for the same schema name, the `-N`
    switch wins and the schema is excluded.

  - <span class="term">`-O`  
    </span><span class="term">`--no-owner`</span>  
    Do not output commands to set ownership of objects to match the
    original database. By default,
    <span class="application">pg\_restore</span> issues `ALTER OWNER` or
    `SET SESSION AUTHORIZATION` statements to set ownership of created
    schema elements. These statements will fail unless the initial
    connection to the database is made by a superuser (or the same user
    that owns all of the objects in the script). With `-O`, any user
    name can be used for the initial connection, and this user will own
    all the created objects.

  - <span class="term">`-P function-name(argtype [, ...])`  
    </span><span class="term">`--function=function-name(argtype [,
    ...])`</span>  
    Restore the named function only. Be careful to spell the function
    name and arguments exactly as they appear in the dump file's table
    of contents. Multiple functions may be specified with multiple `-P`
    switches.

  - <span class="term">`-R`  
    </span><span class="term">`--no-reconnect`</span>  
    This option is obsolete but still accepted for backwards
    compatibility.

  - <span class="term">`-s`  
    </span><span class="term">`--schema-only`</span>  
    Restore only the schema (data definitions), not data, to the extent
    that schema entries are present in the archive.
    
    This option is the inverse of `--data-only`. It is similar to, but
    for historical reasons not identical to, specifying
    `--section=pre-data --section=post-data`.
    
    (Do not confuse this with the `--schema` option, which uses the word
    <span class="quote">“<span class="quote">schema</span>”</span> in a
    different meaning.)

  - <span class="term">`-S username`  
    </span><span class="term">`--superuser=username`</span>  
    Specify the superuser user name to use when disabling triggers. This
    is relevant only if `--disable-triggers` is used.

  - <span class="term">`-t table`  
    </span><span class="term">`--table=table`</span>  
    Restore definition and/or data of only the named table. For this
    purpose,
    <span class="quote">“<span class="quote">table</span>”</span>
    includes views, materialized views, sequences, and foreign tables.
    Multiple tables can be selected by writing multiple `-t` switches.
    This option can be combined with the `-n` option to specify table(s)
    in a particular schema.
    
    <div class="note">
    
    ### Note
    
    When `-t` is specified, <span class="application">pg\_restore</span>
    makes no attempt to restore any other database objects that the
    selected table(s) might depend upon. Therefore, there is no
    guarantee that a specific-table restore into a clean database will
    succeed.
    
    </div>
    
    <div class="note">
    
    ### Note
    
    This flag does not behave identically to the `-t` flag of
    <span class="application">pg\_dump</span>. There is not currently
    any provision for wild-card matching in
    <span class="application">pg\_restore</span>, nor can you include a
    schema name within its `-t`.
    
    </div>
    
    <div class="note">
    
    ### Note
    
    In versions prior to <span class="productname">PostgreSQL</span>
    9.6, this flag matched only tables, not any other type of relation.
    
    </div>

  - <span class="term">`-T trigger`  
    </span><span class="term">`--trigger=trigger`</span>  
    Restore named trigger only. Multiple triggers may be specified with
    multiple `-T` switches.

  - <span class="term">`-v`  
    </span><span class="term">`--verbose`</span>  
    Specifies verbose mode.

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">pg\_restore</span> version and
    exit.

  - <span class="term">`-x`  
    </span><span class="term">`--no-privileges`  
    </span><span class="term">`--no-acl`</span>  
    Prevent restoration of access privileges (grant/revoke commands).

  - <span class="term">`-1`  
    </span><span class="term">`--single-transaction`</span>  
    Execute the restore as a single transaction (that is, wrap the
    emitted commands in `BEGIN`/`COMMIT`). This ensures that either all
    the commands complete successfully, or no changes are applied. This
    option implies `--exit-on-error`.

  - <span class="term">`--disable-triggers`</span>  
    This option is relevant only when performing a data-only restore. It
    instructs <span class="application">pg\_restore</span> to execute
    commands to temporarily disable triggers on the target tables while
    the data is reloaded. Use this if you have referential integrity
    checks or other triggers on the tables that you do not want to
    invoke during data reload.
    
    Presently, the commands emitted for `--disable-triggers` must be
    done as superuser. So you should also specify a superuser name with
    `-S` or, preferably, run
    <span class="application">pg\_restore</span> as a
    <span class="productname">PostgreSQL</span> superuser.

  - <span class="term">`--enable-row-security`</span>  
    This option is relevant only when restoring the contents of a table
    which has row security. By default,
    <span class="application">pg\_restore</span> will set
    [row\_security](runtime-config-client.html#GUC-ROW-SECURITY) to off,
    to ensure that all data is restored in to the table. If the user
    does not have sufficient privileges to bypass row security, then an
    error is thrown. This parameter instructs
    <span class="application">pg\_restore</span> to set
    [row\_security](runtime-config-client.html#GUC-ROW-SECURITY) to on
    instead, allowing the user to attempt to restore the contents of the
    table with row security enabled. This might still fail if the user
    does not have the right to insert the rows from the dump into the
    table.
    
    Note that this option currently also requires the dump be in
    `INSERT` format, as `COPY FROM` does not support row security.

  - <span class="term">`--if-exists`</span>  
    Use conditional commands (i.e. add an `IF EXISTS` clause) when
    cleaning database objects. This option is not valid unless `--clean`
    is also specified.

  - <span class="term">`--no-data-for-failed-tables`</span>  
    By default, table data is restored even if the creation command for
    the table failed (e.g., because it already exists). With this
    option, data for such a table is skipped. This behavior is useful if
    the target database already contains the desired table contents. For
    example, auxiliary tables for
    <span class="productname">PostgreSQL</span> extensions such as
    <span class="productname">PostGIS</span> might already be loaded in
    the target database; specifying this option prevents duplicate or
    obsolete data from being loaded into them.
    
    This option is effective only when restoring directly into a
    database, not when producing SQL script output.

  - <span class="term">`--no-publications`</span>  
    Do not output commands to restore publications, even if the archive
    contains them.

  - <span class="term">`--no-security-labels`</span>  
    Do not output commands to restore security labels, even if the
    archive contains them.

  - <span class="term">`--no-subscriptions`</span>  
    Do not output commands to restore subscriptions, even if the archive
    contains them.

  - <span class="term">`--no-tablespaces`</span>  
    Do not output commands to select tablespaces. With this option, all
    objects will be created in whichever tablespace is the default
    during restore.

  - <span class="term">`--section=sectionname`</span>  
    Only restore the named section. The section name can be `pre-data`,
    `data`, or `post-data`. This option can be specified more than once
    to select multiple sections. The default is to restore all sections.
    
    The data section contains actual table data as well as large-object
    definitions. Post-data items consist of definitions of indexes,
    triggers, rules and constraints other than validated check
    constraints. Pre-data items consist of all other data definition
    items.

  - <span class="term">`--strict-names`</span>  
    Require that each schema (`-n`/`--schema`) and table
    (`-t`/`--table`) qualifier match at least one schema/table in the
    backup file.

  - <span class="term">`--use-set-session-authorization`</span>  
    Output SQL-standard `SET SESSION AUTHORIZATION` commands instead of
    `ALTER OWNER` commands to determine object ownership. This makes the
    dump more standards-compatible, but depending on the history of the
    objects in the dump, might not restore properly.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">pg\_restore</span> command
    line arguments, and exit.

</div>

<span class="application">pg\_restore</span> also accepts the following
command line arguments for connection parameters:

<div class="variablelist">

  - <span class="term">`-h host`  
    </span><span class="term">`--host=host`</span>  
    Specifies the host name of the machine on which the server is
    running. If the value begins with a slash, it is used as the
    directory for the Unix domain socket. The default is taken from the
    `PGHOST` environment variable, if set, else a Unix domain socket
    connection is attempted.

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
    Force <span class="application">pg\_restore</span> to prompt for a
    password before connecting to a database.
    
    This option is never essential, since
    <span class="application">pg\_restore</span> will automatically
    prompt for a password if the server demands password authentication.
    However, <span class="application">pg\_restore</span> will waste a
    connection attempt finding out that the server wants a password. In
    some cases it is worth typing `-W` to avoid the extra connection
    attempt.

  - <span class="term">`--role=rolename`</span>  
    Specifies a role name to be used to perform the restore. This option
    causes <span class="application">pg\_restore</span> to issue a `SET
    ROLE` *`rolename`* command after connecting to the database. It is
    useful when the authenticated user (specified by `-U`) lacks
    privileges needed by <span class="application">pg\_restore</span>,
    but can switch to a role with the required rights. Some
    installations have a policy against logging in directly as a
    superuser, and use of this option allows restores to be performed
    without violating the policy.

</div>

</div>

<div id="id-1.9.4.17.7" class="refsect1">

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
However, it does not read `PGDATABASE` when a database name is not
supplied.

</div>

<div id="APP-PGRESTORE-DIAGNOSTICS" class="refsect1">

## Diagnostics

When a direct database connection is specified using the `-d` option,
<span class="application">pg\_restore</span> internally executes SQL
statements. If you have problems running
<span class="application">pg\_restore</span>, make sure you are able to
select information from the database using, for example,
[<span class="refentrytitle"><span class="application">psql</span></span>](app-psql.html "psql").
Also, any default connection settings and environment variables used by
the <span class="application">libpq</span> front-end library will apply.

</div>

<div id="APP-PGRESTORE-NOTES" class="refsect1">

## Notes

If your installation has any local additions to the `template1`
database, be careful to load the output of
<span class="application">pg\_restore</span> into a truly empty
database; otherwise you are likely to get errors due to duplicate
definitions of the added objects. To make an empty database without any
local additions, copy from `template0` not `template1`, for example:

``` programlisting
CREATE DATABASE foo WITH TEMPLATE template0;
```

The limitations of <span class="application">pg\_restore</span> are
detailed below.

<div class="itemizedlist">

  - When restoring data to a pre-existing table and the option
    `--disable-triggers` is used,
    <span class="application">pg\_restore</span> emits commands to
    disable triggers on user tables before inserting the data, then
    emits commands to re-enable them after the data has been inserted.
    If the restore is stopped in the middle, the system catalogs might
    be left in the wrong state.

  - <span class="application">pg\_restore</span> cannot restore large
    objects selectively; for instance, only those for a specific table.
    If an archive contains large objects, then all large objects will be
    restored, or none of them if they are excluded via `-L`, `-t`, or
    other options.

</div>

See also the
[<span class="refentrytitle">pg\_dump</span>](app-pgdump.html "pg_dump")
documentation for details on limitations of
<span class="application">pg\_dump</span>.

Once restored, it is wise to run `ANALYZE` on each restored table so the
optimizer has useful statistics; see
[Section 24.1.3](routine-vacuuming.html#VACUUM-FOR-STATISTICS "24.1.3. Updating Planner Statistics")
and
[Section 24.1.6](routine-vacuuming.html#AUTOVACUUM "24.1.6. The Autovacuum Daemon")
for more information.

</div>

<div id="APP-PGRESTORE-EXAMPLES" class="refsect1">

## Examples

Assume we have dumped a database called `mydb` into a custom-format dump
file:

``` screen
$ pg_dump -Fc mydb > db.dump
```

To drop the database and recreate it from the dump:

``` screen
$ dropdb mydb
$ pg_restore -C -d postgres db.dump
```

The database named in the `-d` switch can be any database existing in
the cluster; <span class="application">pg\_restore</span> only uses it
to issue the `CREATE DATABASE` command for `mydb`. With `-C`, data is
always restored into the database name that appears in the dump file.

To reload the dump into a new database called `newdb`:

``` screen
$ createdb -T template0 newdb
$ pg_restore -d newdb db.dump
```

Notice we don't use `-C`, and instead connect directly to the database
to be restored into. Also note that we clone the new database from
`template0` not `template1`, to ensure it is initially empty.

To reorder database items, it is first necessary to dump the table of
contents of the archive:

``` screen
$ pg_restore -l db.dump > db.list
```

The listing file consists of a header and one line for each item, e.g.:

``` programlisting
;
; Archive created at Mon Sep 14 13:55:39 2009
;     dbname: DBDEMOS
;     TOC Entries: 81
;     Compression: 9
;     Dump Version: 1.10-0
;     Format: CUSTOM
;     Integer: 4 bytes
;     Offset: 8 bytes
;     Dumped from database version: 8.3.5
;     Dumped by pg_dump version: 8.3.8
;
;
; Selected TOC Entries:
;
3; 2615 2200 SCHEMA - public pasha
1861; 0 0 COMMENT - SCHEMA public pasha
1862; 0 0 ACL - public pasha
317; 1247 17715 TYPE public composite pasha
319; 1247 25899 DOMAIN public domain0 pasha
```

Semicolons start a comment, and the numbers at the start of lines refer
to the internal archive ID assigned to each item.

Lines in the file can be commented out, deleted, and reordered. For
example:

``` programlisting
10; 145433 TABLE map_resolutions postgres
;2; 145344 TABLE species postgres
;4; 145359 TABLE nt_header postgres
6; 145402 TABLE species_records postgres
;8; 145416 TABLE ss_old postgres
```

could be used as input to <span class="application">pg\_restore</span>
and would only restore items 10 and 6, in that
order:

``` screen
$ pg_restore -L db.list db.dump
```

</div>

<div id="id-1.9.4.17.11" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle">pg\_dump</span>](app-pgdump.html "pg_dump"),
[<span class="refentrytitle"><span class="application">pg\_dumpall</span></span>](app-pg-dumpall.html "pg_dumpall"),
[<span class="refentrytitle"><span class="application">psql</span></span>](app-psql.html "psql")</span>

</div>

</div>

<div class="navfooter">

-----

|                                                  |                             |                                       |
| :----------------------------------------------- | :-------------------------: | ------------------------------------: |
| [Prev](app-pgrecvlogical.html)                   | [Up](reference-client.html) |                 [Next](app-psql.html) |
| <span class="application">pg\_recvlogical</span> |     [Home](index.html)      | <span class="application">psql</span> |

</div>
