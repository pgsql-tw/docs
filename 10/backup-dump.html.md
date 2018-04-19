<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

25.1. SQL Dump

</div>

[Prev](backup.html "Chapter 25. Backup and Restore") 

[Up](backup.html "Chapter 25. Backup and Restore")

Chapter 25. Backup and Restore

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](backup-file.html "25.2. File System Level Backup")

-----

<div id="BACKUP-DUMP" class="sect1">

<div class="titlepage">

<div>

<div>

## 25.1. SQL Dump

</div>

</div>

</div>

<div class="toc">

<span class="sect2">[25.1.1. Restoring the
Dump](backup-dump.html#BACKUP-DUMP-RESTORE)</span>

<span class="sect2">[25.1.2. Using
<span class="application">pg\_dumpall</span>](backup-dump.html#BACKUP-DUMP-ALL)</span>

<span class="sect2">[25.1.3. Handling Large
Databases](backup-dump.html#BACKUP-DUMP-LARGE)</span>

</div>

The idea behind this dump method is to generate a file with SQL commands
that, when fed back to the server, will recreate the database in the
same state as it was at the time of the dump.
<span class="productname">PostgreSQL</span> provides the utility program
[<span class="refentrytitle">pg\_dump</span>](app-pgdump.html "pg_dump")
for this purpose. The basic usage of this command is:

``` synopsis
pg_dump dbname > outfile
```

As you see, <span class="application">pg\_dump</span> writes its result
to the standard output. We will see below how this can be useful. While
the above command creates a text file,
<span class="application">pg\_dump</span> can create files in other
formats that allow for parallelism and more fine-grained control of
object restoration.

<span class="application">pg\_dump</span> is a regular
<span class="productname">PostgreSQL</span> client application (albeit a
particularly clever one). This means that you can perform this backup
procedure from any remote host that has access to the database. But
remember that <span class="application">pg\_dump</span> does not operate
with special permissions. In particular, it must have read access to all
tables that you want to back up, so in order to back up the entire
database you almost always have to run it as a database superuser. (If
you do not have sufficient privileges to back up the entire database,
you can still back up portions of the database to which you do have
access using options such as `-n schema` or `-t table`.)

To specify which database server
<span class="application">pg\_dump</span> should contact, use the
command line options `-h host` and `-p port`. The default host is the
local host or whatever your `PGHOST` environment variable specifies.
Similarly, the default port is indicated by the `PGPORT` environment
variable or, failing that, by the compiled-in default. (Conveniently,
the server will normally have the same compiled-in default.)

Like any other <span class="productname">PostgreSQL</span> client
application, <span class="application">pg\_dump</span> will by default
connect with the database user name that is equal to the current
operating system user name. To override this, either specify the `-U`
option or set the environment variable `PGUSER`. Remember that
<span class="application">pg\_dump</span> connections are subject to the
normal client authentication mechanisms (which are described in
[Chapter 20](client-authentication.html "Chapter 20. Client Authentication")).

An important advantage of <span class="application">pg\_dump</span> over
the other backup methods described later is that
<span class="application">pg\_dump</span>'s output can generally be
re-loaded into newer versions of
<span class="productname">PostgreSQL</span>, whereas file-level backups
and continuous archiving are both extremely server-version-specific.
<span class="application">pg\_dump</span> is also the only method that
will work when transferring a database to a different machine
architecture, such as going from a 32-bit to a 64-bit server.

Dumps created by <span class="application">pg\_dump</span> are
internally consistent, meaning, the dump represents a snapshot of the
database at the time <span class="application">pg\_dump</span> began
running. <span class="application">pg\_dump</span> does not block other
operations on the database while it is working. (Exceptions are those
operations that need to operate with an exclusive lock, such as most
forms of `ALTER TABLE`.)

<div id="BACKUP-DUMP-RESTORE" class="sect2">

<div class="titlepage">

<div>

<div>

### 25.1.1. Restoring the Dump

</div>

</div>

</div>

Text files created by <span class="application">pg\_dump</span> are
intended to be read in by the <span class="application">psql</span>
program. The general command form to restore a dump is

``` synopsis
psql dbname < infile
```

where *`infile`* is the file output by the
<span class="application">pg\_dump</span> command. The database
*`dbname`* will not be created by this command, so you must create it
yourself from `template0` before executing
<span class="application">psql</span> (e.g., with `createdb -T template0
dbname`). <span class="application">psql</span> supports options similar
to <span class="application">pg\_dump</span> for specifying the database
server to connect to and the user name to use. See the
[<span class="refentrytitle"><span class="application">psql</span></span>](app-psql.html "psql")
reference page for more information. Non-text file dumps are restored
using the
[<span class="refentrytitle">pg\_restore</span>](app-pgrestore.html "pg_restore")
utility.

Before restoring an SQL dump, all the users who own objects or were
granted permissions on objects in the dumped database must already
exist. If they do not, the restore will fail to recreate the objects
with the original ownership and/or permissions. (Sometimes this is what
you want, but usually it is not.)

By default, the <span class="application">psql</span> script will
continue to execute after an SQL error is encountered. You might wish to
run <span class="application">psql</span> with the `ON_ERROR_STOP`
variable set to alter that behavior and have
<span class="application">psql</span> exit with an exit status of 3 if
an SQL error occurs:

``` programlisting
psql --set ON_ERROR_STOP=on dbname < infile
```

Either way, you will only have a partially restored database.
Alternatively, you can specify that the whole dump should be restored as
a single transaction, so the restore is either fully completed or fully
rolled back. This mode can be specified by passing the `-1` or
`--single-transaction` command-line options to
<span class="application">psql</span>. When using this mode, be aware
that even a minor error can rollback a restore that has already run for
many hours. However, that might still be preferable to manually cleaning
up a complex database after a partially restored dump.

The ability of <span class="application">pg\_dump</span> and
<span class="application">psql</span> to write to or read from pipes
makes it possible to dump a database directly from one server to
another, for example:

``` programlisting
pg_dump -h host1 dbname | psql -h host2 dbname
```

<div class="important">

### Important

The dumps produced by <span class="application">pg\_dump</span> are
relative to `template0`. This means that any languages, procedures, etc.
added via `template1` will also be dumped by
<span class="application">pg\_dump</span>. As a result, when restoring,
if you are using a customized `template1`, you must create the empty
database from `template0`, as in the example above.

</div>

After restoring a backup, it is wise to run
[<span class="refentrytitle">ANALYZE</span>](sql-analyze.html "ANALYZE")
on each database so the query optimizer has useful statistics; see
[Section 24.1.3](routine-vacuuming.html#VACUUM-FOR-STATISTICS "24.1.3. Updating Planner Statistics")
and
[Section 24.1.6](routine-vacuuming.html#AUTOVACUUM "24.1.6. The Autovacuum Daemon")
for more information. For more advice on how to load large amounts of
data into <span class="productname">PostgreSQL</span> efficiently, refer
to [Section 14.4](populate.html "14.4. Populating a Database").

</div>

<div id="BACKUP-DUMP-ALL" class="sect2">

<div class="titlepage">

<div>

<div>

### 25.1.2. Using <span class="application">pg\_dumpall</span>

</div>

</div>

</div>

<span class="application">pg\_dump</span> dumps only a single database
at a time, and it does not dump information about roles or tablespaces
(because those are cluster-wide rather than per-database). To support
convenient dumping of the entire contents of a database cluster, the
[<span class="refentrytitle"><span class="application">pg\_dumpall</span></span>](app-pg-dumpall.html "pg_dumpall")
program is provided. <span class="application">pg\_dumpall</span> backs
up each database in a given cluster, and also preserves cluster-wide
data such as role and tablespace definitions. The basic usage of this
command is:

``` synopsis
pg_dumpall > outfile
```

The resulting dump can be restored with
<span class="application">psql</span>:

``` synopsis
psql -f infile postgres
```

(Actually, you can specify any existing database name to start from, but
if you are loading into an empty cluster then `postgres` should usually
be used.) It is always necessary to have database superuser access when
restoring a <span class="application">pg\_dumpall</span> dump, as that
is required to restore the role and tablespace information. If you use
tablespaces, make sure that the tablespace paths in the dump are
appropriate for the new installation.

<span class="application">pg\_dumpall</span> works by emitting commands
to re-create roles, tablespaces, and empty databases, then invoking
<span class="application">pg\_dump</span> for each database. This means
that while each database will be internally consistent, the snapshots of
different databases are not synchronized.

Cluster-wide data can be dumped alone using the
<span class="application">pg\_dumpall</span> `--globals-only` option.
This is necessary to fully backup the cluster if running the
<span class="application">pg\_dump</span> command on individual
databases.

</div>

<div id="BACKUP-DUMP-LARGE" class="sect2">

<div class="titlepage">

<div>

<div>

### 25.1.3. Handling Large Databases

</div>

</div>

</div>

Some operating systems have maximum file size limits that cause problems
when creating large <span class="application">pg\_dump</span> output
files. Fortunately, <span class="application">pg\_dump</span> can write
to the standard output, so you can use standard Unix tools to work
around this potential problem. There are several possible methods:

**Use compressed dumps. ** You can use your favorite compression
program, for example <span class="application">gzip</span>:

``` programlisting
pg_dump dbname | gzip > filename.gz
```

Reload with:

``` programlisting
gunzip -c filename.gz | psql dbname
```

or:

``` programlisting
cat filename.gz | gunzip | psql dbname
```

**Use `split`. ** The `split` command allows you to split the output
into smaller files that are acceptable in size to the underlying file
system. For example, to make chunks of 1 megabyte:

``` programlisting
pg_dump dbname | split -b 1m - filename
```

Reload with:

``` programlisting
cat filename* | psql dbname
```

**Use <span class="application">pg\_dump</span>'s custom dump format. **
If <span class="productname">PostgreSQL</span> was built on a system
with the <span class="application">zlib</span> compression library
installed, the custom dump format will compress data as it writes it to
the output file. This will produce dump file sizes similar to using
`gzip`, but it has the added advantage that tables can be restored
selectively. The following command dumps a database using the custom
dump format:

``` programlisting
pg_dump -Fc dbname > filename
```

A custom-format dump is not a script for
<span class="application">psql</span>, but instead must be restored with
<span class="application">pg\_restore</span>, for example:

``` programlisting
pg_restore -d dbname filename
```

See the
[<span class="refentrytitle">pg\_dump</span>](app-pgdump.html "pg_dump")
and
[<span class="refentrytitle">pg\_restore</span>](app-pgrestore.html "pg_restore")
reference pages for details.

For very large databases, you might need to combine `split` with one of
the other two approaches.

**Use <span class="application">pg\_dump</span>'s parallel dump
feature. ** To speed up the dump of a large database, you can use
<span class="application">pg\_dump</span>'s parallel mode. This will
dump multiple tables at the same time. You can control the degree of
parallelism with the `-j` parameter. Parallel dumps are only supported
for the "directory" archive format.

``` programlisting
pg_dump -j num -F d -f out.dir dbname
```

You can use `pg_restore -j` to restore a dump in parallel. This will
work for any archive of either the "custom" or the "directory" archive
mode, whether or not it has been created with `pg_dump
-j`.

</div>

</div>

<div class="navfooter">

-----

|                                |                    |                                |
| :----------------------------- | :----------------: | -----------------------------: |
| [Prev](backup.html)            | [Up](backup.html)  |       [Next](backup-file.html) |
| Chapter 25. Backup and Restore | [Home](index.html) | 25.2. File System Level Backup |

</div>
