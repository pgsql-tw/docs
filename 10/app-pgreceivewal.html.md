<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

pg\_receivewal

</div>

[Prev](app-pg-isready.html "pg_isready") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-pgrecvlogical.html "pg_recvlogical")

-----

<div id="APP-PGRECEIVEWAL" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.15.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle">pg\_receivewal</span>

pg\_receivewal — stream write-ahead logs from a
<span class="productname">PostgreSQL</span> server

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`pg_receivewal` \[*`option`*...\]

</div>

</div>

<div id="id-1.9.4.15.5" class="refsect1">

## Description

<span class="application">pg\_receivewal</span> is used to stream the
write-ahead log from a running
<span class="productname">PostgreSQL</span> cluster. The write-ahead log
is streamed using the streaming replication protocol, and is written to
a local directory of files. This directory can be used as the archive
location for doing a restore using point-in-time recovery (see
[Section 25.3](continuous-archiving.html "25.3. Continuous Archiving and Point-in-Time Recovery (PITR)")).

<span class="application">pg\_receivewal</span> streams the write-ahead
log in real time as it's being generated on the server, and does not
wait for segments to complete like
[archive\_command](runtime-config-wal.html#GUC-ARCHIVE-COMMAND) does.
For this reason, it is not necessary to set
[archive\_timeout](runtime-config-wal.html#GUC-ARCHIVE-TIMEOUT) when
using <span class="application">pg\_receivewal</span>.

Unlike the WAL receiver of a PostgreSQL standby server,
<span class="application">pg\_receivewal</span> by default flushes WAL
data only when a WAL file is closed. The option `--synchronous` must be
specified to flush WAL data in real time.

The write-ahead log is streamed over a regular
<span class="productname">PostgreSQL</span> connection and uses the
replication protocol. The connection must be made with a superuser or a
user having `REPLICATION` permissions (see
[Section 21.2](role-attributes.html "21.2. Role Attributes")), and
`pg_hba.conf` must permit the replication connection. The server must
also be configured with
[max\_wal\_senders](runtime-config-replication.html#GUC-MAX-WAL-SENDERS)
set high enough to leave at least one session available for the stream.

If the connection is lost, or if it cannot be initially established,
with a non-fatal error, <span class="application">pg\_receivewal</span>
will retry the connection indefinitely, and reestablish streaming as
soon as possible. To avoid this behavior, use the `-n` parameter.

</div>

<div id="id-1.9.4.15.6" class="refsect1">

## Options

<div class="variablelist">

  - <span class="term">`-D directory`  
    </span><span class="term">`--directory=directory`</span>  
    Directory to write the output to.
    
    This parameter is required.

  - <span class="term">`--if-not-exists`</span>  
    Do not error out when `--create-slot` is specified and a slot with
    the specified name already exists.

  - <span class="term">`-n`  
    </span><span class="term">`--no-loop`</span>  
    Don't loop on connection errors. Instead, exit right away with an
    error.

  - <span class="term">`-s interval`  
    </span><span class="term">`--status-interval=interval`</span>  
    Specifies the number of seconds between status packets sent back to
    the server. This allows for easier monitoring of the progress from
    server. A value of zero disables the periodic status updates
    completely, although an update will still be sent when requested by
    the server, to avoid timeout disconnect. The default value is 10
    seconds.

  - <span class="term">`-S slotname`  
    </span><span class="term">`--slot=slotname`</span>  
    Require <span class="application">pg\_receivewal</span> to use an
    existing replication slot (see
    [Section 26.2.6](warm-standby.html#STREAMING-REPLICATION-SLOTS "26.2.6. Replication Slots")).
    When this option is used,
    <span class="application">pg\_receivewal</span> will report a flush
    position to the server, indicating when each segment has been
    synchronized to disk so that the server can remove that segment if
    it is not otherwise needed.
    
    When the replication client of
    <span class="application">pg\_receivewal</span> is configured on the
    server as a synchronous standby, then using a replication slot will
    report the flush position to the server, but only when a WAL file is
    closed. Therefore, that configuration will cause transactions on the
    primary to wait for a long time and effectively not work
    satisfactorily. The option `--synchronous` (see below) must be
    specified in addition to make this work correctly.

  - <span class="term">`--synchronous`</span>  
    Flush the WAL data to disk immediately after it has been received.
    Also send a status packet back to the server immediately after
    flushing, regardless of `--status-interval`.
    
    This option should be specified if the replication client of
    <span class="application">pg\_receivewal</span> is configured on the
    server as a synchronous standby, to ensure that timely feedback is
    sent to the server.

  - <span class="term">`-v`  
    </span><span class="term">`--verbose`</span>  
    Enables verbose mode.

  - <span class="term">`-Z level`  
    </span><span class="term">`--compress=level`</span>  
    Enables gzip compression of write-ahead logs, and specifies the
    compression level (0 through 9, 0 being no compression and 9 being
    best compression). The suffix `.gz` will automatically be added to
    all filenames.

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
    <span class="application">pg\_receivewal</span> doesn't connect to
    any particular database in the cluster, database name in the
    connection string will be ignored.

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
    Force <span class="application">pg\_receivewal</span> to prompt for
    a password before connecting to a database.
    
    This option is never essential, since
    <span class="application">pg\_receivewal</span> will automatically
    prompt for a password if the server demands password authentication.
    However, <span class="application">pg\_receivewal</span> will waste
    a connection attempt finding out that the server wants a password.
    In some cases it is worth typing `-W` to avoid the extra connection
    attempt.

</div>

<span class="application">pg\_receivewal</span> can perform one of the
two following actions in order to control physical replication slots:

<div class="variablelist">

  - <span class="term">`--create-slot`</span>  
    Create a new physical replication slot with the name specified in
    `--slot`, then exit.

  - <span class="term">`--drop-slot`</span>  
    Drop the replication slot with the name specified in `--slot`, then
    exit.

</div>

Other options are also available:

<div class="variablelist">

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">pg\_receivewal</span> version
    and exit.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">pg\_receivewal</span>
    command line arguments, and exit.

</div>

</div>

<div id="id-1.9.4.15.7" class="refsect1">

## Environment

This utility, like most other
<span class="productname">PostgreSQL</span> utilities, uses the
environment variables supported by
<span class="application">libpq</span> (see
[Section 33.14](libpq-envars.html "33.14. Environment Variables")).

</div>

<div id="id-1.9.4.15.8" class="refsect1">

## Notes

When using <span class="application">pg\_receivewal</span> instead of
[archive\_command](runtime-config-wal.html#GUC-ARCHIVE-COMMAND) as the
main WAL backup method, it is strongly recommended to use replication
slots. Otherwise, the server is free to recycle or remove write-ahead
log files before they are backed up, because it does not have any
information, either from
[archive\_command](runtime-config-wal.html#GUC-ARCHIVE-COMMAND) or the
replication slots, about how far the WAL stream has been archived. Note,
however, that a replication slot will fill up the server's disk space if
the receiver does not keep up with fetching the WAL data.

</div>

<div id="id-1.9.4.15.9" class="refsect1">

## Examples

To stream the write-ahead log from the server at `mydbserver` and store
it in the local directory
`/usr/local/pgsql/archive`:

``` screen
$ pg_receivewal -h mydbserver -D /usr/local/pgsql/archive
```

</div>

<div id="id-1.9.4.15.10" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle">pg\_basebackup</span>](app-pgbasebackup.html "pg_basebackup")</span>

</div>

</div>

<div class="navfooter">

-----

|                                              |                             |                                                  |
| :------------------------------------------- | :-------------------------: | -----------------------------------------------: |
| [Prev](app-pg-isready.html)                  | [Up](reference-client.html) |                   [Next](app-pgrecvlogical.html) |
| <span class="application">pg\_isready</span> |     [Home](index.html)      | <span class="application">pg\_recvlogical</span> |

</div>
