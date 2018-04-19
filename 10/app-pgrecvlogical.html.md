<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">pg\_recvlogical</span>

</div>

[Prev](app-pgreceivewal.html "pg_receivewal") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-pgrestore.html "pg_restore")

-----

<div id="APP-PGRECVLOGICAL" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.16.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">pg\_recvlogical</span></span>

pg\_recvlogical — control <span class="productname">PostgreSQL</span>
logical decoding streams

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`pg_recvlogical` \[*`option`*...\]

</div>

</div>

<div id="id-1.9.4.16.5" class="refsect1">

## Description

`pg_recvlogical` controls logical decoding replication slots and streams
data from such replication slots.

It creates a replication-mode connection, so it is subject to the same
constraints as
[<span class="refentrytitle">pg\_receivewal</span>](app-pgreceivewal.html "pg_receivewal"),
plus those for logical replication (see
[Chapter 48](logicaldecoding.html "Chapter 48. Logical Decoding")).

`pg_recvlogical` has no equivalent to the logical decoding SQL
interface's peek and get modes. It sends replay confirmations for data
lazily as it receives it and on clean exit. To examine pending data on a
slot without consuming it, use
[`pg_logical_slot_peek_changes`](functions-admin.html#FUNCTIONS-REPLICATION "9.26.6. Replication Functions").

</div>

<div id="id-1.9.4.16.6" class="refsect1">

## Options

At least one of the following options must be specified to select an
action:

<div class="variablelist">

  - <span class="term">`--create-slot`</span>  
    Create a new logical replication slot with the name specified by
    `--slot`, using the output plugin specified by `--plugin`, for the
    database specified by `--dbname`.

  - <span class="term">`--drop-slot`</span>  
    Drop the replication slot with the name specified by `--slot`, then
    exit.

  - <span class="term">`--start`</span>  
    Begin streaming changes from the logical replication slot specified
    by `--slot`, continuing until terminated by a signal. If the server
    side change stream ends with a server shutdown or disconnect, retry
    in a loop unless `--no-loop` is specified.
    
    The stream format is determined by the output plugin specified when
    the slot was created.
    
    The connection must be to the same database used to create the slot.

</div>

`--create-slot` and `--start` can be specified together. `--drop-slot`
cannot be combined with another action.

The following command-line options control the location and format of
the output and other replication behavior:

<div class="variablelist">

  - <span class="term">`-E lsn`  
    </span><span class="term">`--endpos=lsn`</span>  
    In `--start` mode, automatically stop replication and exit with
    normal exit status 0 when receiving reaches the specified LSN. If
    specified when not in `--start` mode, an error is raised.
    
    If there's a record with LSN exactly equal to *`lsn`*, the record
    will be output.
    
    The `--endpos` option is not aware of transaction boundaries and may
    truncate output partway through a transaction. Any partially output
    transaction will not be consumed and will be replayed again when the
    slot is next read from. Individual messages are never truncated.

  - <span class="term">`-f filename`  
    </span><span class="term">`--file=filename`</span>  
    Write received and decoded transaction data into this file. Use `-`
    for <span class="systemitem">stdout</span>.

  - <span class="term">`-F
    interval_seconds`  
    </span><span class="term">`--fsync-interval=interval_seconds`</span>  
    Specifies how often <span class="application">pg\_recvlogical</span>
    should issue `fsync()` calls to ensure the output file is safely
    flushed to disk.
    
    The server will occasionally request the client to perform a flush
    and report the flush position to the server. This setting is in
    addition to that, to perform flushes more frequently.
    
    Specifying an interval of `0` disables issuing `fsync()` calls
    altogether, while still reporting progress to the server. In this
    case, data could be lost in the event of a crash.

  - <span class="term">`-I lsn`  
    </span><span class="term">`--startpos=lsn`</span>  
    In `--start` mode, start replication from the given LSN. For details
    on the effect of this, see the documentation in
    [Chapter 48](logicaldecoding.html "Chapter 48. Logical Decoding")
    and
    [Section 52.4](protocol-replication.html "52.4. Streaming Replication Protocol").
    Ignored in other modes.

  - <span class="term">`--if-not-exists`</span>  
    Do not error out when `--create-slot` is specified and a slot with
    the specified name already exists.

  - <span class="term">`-n`  
    </span><span class="term">`--no-loop`</span>  
    When the connection to the server is lost, do not retry in a loop,
    just exit.

  - <span class="term">`-o name`\[=*`value`*\]  
    </span><span class="term">`--option=name`\[=*`value`*\]</span>  
    Pass the option *`name`* to the output plugin with, if specified,
    the option value *`value`*. Which options exist and their effects
    depends on the used output plugin.

  - <span class="term">`-P plugin`  
    </span><span class="term">`--plugin=plugin`</span>  
    When creating a slot, use the specified logical decoding output
    plugin. See
    [Chapter 48](logicaldecoding.html "Chapter 48. Logical Decoding").
    This option has no effect if the slot already exists.

  - <span class="term">`-s
    interval_seconds`  
    </span><span class="term">`--status-interval=interval_seconds`</span>  
    This option has the same effect as the option of the same name in
    [<span class="refentrytitle">pg\_receivewal</span>](app-pgreceivewal.html "pg_receivewal").
    See the description there.

  - <span class="term">`-S slot_name`  
    </span><span class="term">`--slot=slot_name`</span>  
    In `--start` mode, use the existing logical replication slot named
    *`slot_name`*. In `--create-slot` mode, create the slot with this
    name. In `--drop-slot` mode, delete the slot with this name.

  - <span class="term">`-v`  
    </span><span class="term">`--verbose`</span>  
    Enables verbose mode.

</div>

The following command-line options control the database connection
parameters.

<div class="variablelist">

  - <span class="term">`-d database`  
    </span><span class="term">`--dbname=database`</span>  
    The database to connect to. See the description of the actions for
    what this means in detail. This can be a
    <span class="application">libpq</span> connection string; see
    [Section 33.1.1](libpq-connect.html#LIBPQ-CONNSTRING "33.1.1. Connection Strings")
    for more information. Defaults to user name.

  - <span class="term">`-h hostname-or-ip`  
    </span><span class="term">`--host=hostname-or-ip`</span>  
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

  - <span class="term">`-U user`  
    </span><span class="term">`--username=user`</span>  
    User name to connect as. Defaults to current operating system user
    name.

  - <span class="term">`-w`  
    </span><span class="term">`--no-password`</span>  
    Never issue a password prompt. If the server requires password
    authentication and a password is not available by other means such
    as a `.pgpass` file, the connection attempt will fail. This option
    can be useful in batch jobs and scripts where no user is present to
    enter a password.

  - <span class="term">`-W`  
    </span><span class="term">`--password`</span>  
    Force <span class="application">pg\_recvlogical</span> to prompt for
    a password before connecting to a database.
    
    This option is never essential, since
    <span class="application">pg\_recvlogical</span> will automatically
    prompt for a password if the server demands password authentication.
    However, <span class="application">pg\_recvlogical</span> will waste
    a connection attempt finding out that the server wants a password.
    In some cases it is worth typing `-W` to avoid the extra connection
    attempt.

</div>

The following additional options are available:

<div class="variablelist">

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">pg\_recvlogical</span> version
    and exit.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">pg\_recvlogical</span>
    command line arguments, and exit.

</div>

</div>

<div id="id-1.9.4.16.7" class="refsect1">

## Environment

This utility, like most other
<span class="productname">PostgreSQL</span> utilities, uses the
environment variables supported by
<span class="application">libpq</span> (see
[Section 33.14](libpq-envars.html "33.14. Environment Variables")).

</div>

<div id="id-1.9.4.16.8" class="refsect1">

## Examples

See
[Section 48.1](logicaldecoding-example.html "48.1. Logical Decoding Examples")
for an
example.

</div>

<div id="id-1.9.4.16.9" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle">pg\_receivewal</span>](app-pgreceivewal.html "pg_receivewal")</span>

</div>

</div>

<div class="navfooter">

-----

|                               |                             |                            |
| :---------------------------- | :-------------------------: | -------------------------: |
| [Prev](app-pgreceivewal.html) | [Up](reference-client.html) | [Next](app-pgrestore.html) |
| pg\_receivewal                |     [Home](index.html)      |                pg\_restore |

</div>
