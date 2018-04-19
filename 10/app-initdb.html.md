<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

initdb

</div>

[Prev](reference-server.html "PostgreSQL Server Applications") 

[Up](reference-server.html "PostgreSQL Server Applications")

PostgreSQL Server Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](pgarchivecleanup.html "pg_archivecleanup")

-----

<div id="APP-INITDB" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.5.3.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle">initdb</span>

initdb — create a new <span class="productname">PostgreSQL</span>
database cluster

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`initdb` \[*`option`*...\] \[ `--pgdata` | `-D` \]*` directory`*

</div>

</div>

<div id="R1-APP-INITDB-1" class="refsect1">

## Description

`initdb` creates a new <span class="productname">PostgreSQL</span>
database cluster. A database cluster is a collection of databases that
are managed by a single server instance.

Creating a database cluster consists of creating the directories in
which the database data will live, generating the shared catalog tables
(tables that belong to the whole cluster rather than to any particular
database), and creating the `template1` and `postgres` databases. When
you later create a new database, everything in the `template1` database
is copied. (Therefore, anything installed in `template1` is
automatically copied into each database created later.) The `postgres`
database is a default database meant for use by users, utilities and
third party applications.

Although `initdb` will attempt to create the specified data directory,
it might not have permission if the parent directory of the desired data
directory is root-owned. To initialize in such a setup, create an empty
data directory as root, then use `chown` to assign ownership of that
directory to the database user account, then `su` to become the database
user to run `initdb`.

`initdb` must be run as the user that will own the server process,
because the server needs to have access to the files and directories
that `initdb` creates. Since the server cannot be run as root, you must
not run `initdb` as root either. (It will in fact refuse to do so.)

`initdb` initializes the database cluster's default locale and character
set encoding. The character set encoding, collation order (`LC_COLLATE`)
and character set classes (`LC_CTYPE`, e.g. upper, lower, digit) can be
set separately for a database when it is created. `initdb` determines
those settings for the `template1` database, which will serve as the
default for all other databases.

To alter the default collation order or character set classes, use the
`--lc-collate` and `--lc-ctype` options. Collation orders other than `C`
or `POSIX` also have a performance penalty. For these reasons it is
important to choose the right locale when running `initdb`.

The remaining locale categories can be changed later when the server is
started. You can also use `--locale` to set the default for all locale
categories, including collation order and character set classes. All
server locale values (`lc_*`) can be displayed via `SHOW ALL`. More
details can be found in
[Section 23.1](locale.html "23.1. Locale Support").

To alter the default encoding, use the `--encoding`. More details can be
found in [Section 23.3](multibyte.html "23.3. Character Set Support").

</div>

<div id="id-1.9.5.3.6" class="refsect1">

## Options

<div class="variablelist">

  - <span class="term">`-A authmethod`  
    </span><span class="term">`--auth=authmethod`</span>  
    This option specifies the default authentication method for local
    users used in `pg_hba.conf` (`host` and `local` lines). `initdb`
    will prepopulate `pg_hba.conf` entries using the specified
    authentication method for non-replication as well as replication
    connections.
    
    Do not use `trust` unless you trust all local users on your system.
    `trust` is the default for ease of installation.

  - <span class="term">`--auth-host=authmethod`</span>  
    This option specifies the authentication method for local users via
    TCP/IP connections used in `pg_hba.conf` (`host` lines).

  - <span class="term">`--auth-local=authmethod`</span>  
    This option specifies the authentication method for local users via
    Unix-domain socket connections used in `pg_hba.conf` (`local`
    lines).

  - <span class="term">`-D directory`  
    </span><span class="term">`--pgdata=directory`</span>  
    This option specifies the directory where the database cluster
    should be stored. This is the only information required by `initdb`,
    but you can avoid writing it by setting the `PGDATA` environment
    variable, which can be convenient since the database server
    (`postgres`) can find the database directory later by the same
    variable.

  - <span class="term">`-E encoding`  
    </span><span class="term">`--encoding=encoding`</span>  
    Selects the encoding of the template database. This will also be the
    default encoding of any database you create later, unless you
    override it there. The default is derived from the locale, or
    `SQL_ASCII` if that does not work. The character sets supported by
    the <span class="productname">PostgreSQL</span> server are described
    in
    [Section 23.3.1](multibyte.html#MULTIBYTE-CHARSET-SUPPORTED "23.3.1. Supported Character Sets").

  - <span class="term">`-k`  
    </span><span class="term">`--data-checksums`</span>  
    Use checksums on data pages to help detect corruption by the I/O
    system that would otherwise be silent. Enabling checksums may incur
    a noticeable performance penalty. This option can only be set during
    initialization, and cannot be changed later. If set, checksums are
    calculated for all objects, in all databases.

  - <span class="term">`--locale=locale`</span>  
    Sets the default locale for the database cluster. If this option is
    not specified, the locale is inherited from the environment that
    `initdb` runs in. Locale support is described in
    [Section 23.1](locale.html "23.1. Locale Support").

  - <span class="term">`--lc-collate=locale`  
    </span><span class="term">`--lc-ctype=locale`  
    </span><span class="term">`--lc-messages=locale`  
    </span><span class="term">`--lc-monetary=locale`  
    </span><span class="term">`--lc-numeric=locale`  
    </span><span class="term">`--lc-time=locale`</span>  
    Like `--locale`, but only sets the locale in the specified category.

  - <span class="term">`--no-locale`</span>  
    Equivalent to `--locale=C`.

  - <span class="term">`-N`  
    </span><span class="term">`--no-sync`</span>  
    By default, `initdb` will wait for all files to be written safely to
    disk. This option causes `initdb` to return without waiting, which
    is faster, but means that a subsequent operating system crash can
    leave the data directory corrupt. Generally, this option is useful
    for testing, but should not be used when creating a production
    installation.

  - <span class="term">`--pwfile=filename`</span>  
    Makes `initdb` read the database superuser's password from a file.
    The first line of the file is taken as the password.

  - <span class="term">`-S`  
    </span><span class="term">`--sync-only`</span>  
    Safely write all database files to disk and exit. This does not
    perform any of the normal <span class="application">initdb</span>
    operations.

  - <span class="term">`-T config`  
    </span><span class="term">`--text-search-config=config`</span>  
    Sets the default text search configuration. See
    [default\_text\_search\_config](runtime-config-client.html#GUC-DEFAULT-TEXT-SEARCH-CONFIG)
    for further information.

  - <span class="term">`-U username`  
    </span><span class="term">`--username=username`</span>  
    Selects the user name of the database superuser. This defaults to
    the name of the effective user running `initdb`. It is really not
    important what the superuser's name is, but one might choose to keep
    the customary name <span class="systemitem">postgres</span>, even if
    the operating system user's name is different.

  - <span class="term">`-W`  
    </span><span class="term">`--pwprompt`</span>  
    Makes `initdb` prompt for a password to give the database superuser.
    If you don't plan on using password authentication, this is not
    important. Otherwise you won't be able to use password
    authentication until you have a password set up.

  - <span class="term">`-X directory`  
    </span><span class="term">`--waldir=directory`</span>  
    This option specifies the directory where the write-ahead log should
    be stored.

</div>

Other, less commonly used, options are also available:

<div class="variablelist">

  - <span class="term">`-d`  
    </span><span class="term">`--debug`</span>  
    Print debugging output from the bootstrap backend and a few other
    messages of lesser interest for the general public. The bootstrap
    backend is the program `initdb` uses to create the catalog tables.
    This option generates a tremendous amount of extremely boring
    output.

  - <span class="term">`-L directory`</span>  
    Specifies where `initdb` should find its input files to initialize
    the database cluster. This is normally not necessary. You will be
    told if you need to specify their location explicitly.

  - <span class="term">`-n`  
    </span><span class="term">`--no-clean`</span>  
    By default, when `initdb` determines that an error prevented it from
    completely creating the database cluster, it removes any files it
    might have created before discovering that it cannot finish the job.
    This option inhibits tidying-up and is thus useful for debugging.

</div>

Other options:

<div class="variablelist">

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">initdb</span> version and exit.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">initdb</span> command line
    arguments, and exit.

</div>

</div>

<div id="id-1.9.5.3.7" class="refsect1">

## Environment

<div class="variablelist">

  - <span class="term">`PGDATA`</span>  
    Specifies the directory where the database cluster is to be stored;
    can be overridden using the `-D` option.

  - <span class="term">`TZ`</span>  
    Specifies the default time zone of the created database cluster. The
    value should be a full time zone name (see
    [Section 8.5.3](datatype-datetime.html#DATATYPE-TIMEZONES "8.5.3. Time Zones")).

</div>

This utility, like most other
<span class="productname">PostgreSQL</span> utilities, also uses the
environment variables supported by
<span class="application">libpq</span> (see
[Section 33.14](libpq-envars.html "33.14. Environment Variables")).

</div>

<div id="id-1.9.5.3.8" class="refsect1">

## Notes

`initdb` can also be invoked via `pg_ctl
initdb`.

</div>

<div id="id-1.9.5.3.9" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle"><span class="application">pg\_ctl</span></span>](app-pg-ctl.html "pg_ctl"),
[<span class="refentrytitle"><span class="application">postgres</span></span>](app-postgres.html "postgres")</span>

</div>

</div>

<div class="navfooter">

-----

|                                |                             |                                                     |
| :----------------------------- | :-------------------------: | --------------------------------------------------: |
| [Prev](reference-server.html)  | [Up](reference-server.html) |                       [Next](pgarchivecleanup.html) |
| PostgreSQL Server Applications |     [Home](index.html)      | <span class="application">pg\_archivecleanup</span> |

</div>
