<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">pg\_ctl</span>

</div>

[Prev](app-pgcontroldata.html "pg_controldata") 

[Up](reference-server.html "PostgreSQL Server Applications")

PostgreSQL Server
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-pgresetwal.html "pg_resetwal")

-----

<div id="APP-PG-CTL" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.5.6.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">pg\_ctl</span></span>

pg\_ctl — initialize, start, stop, or control a
<span class="productname">PostgreSQL</span> server

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`pg_ctl` `init[db]` \[`-D` *`datadir`*\] \[`-s`\] \[`-o`
*`initdb-options`*\]

</div>

<div class="cmdsynopsis">

`pg_ctl` `start` \[`-D` *`datadir`*\] \[`-l` *`filename`*\] \[`-W`\]
\[`-t` *`seconds`*\] \[`-s`\] \[`-o` *`options`*\] \[`-p` *`path`*\]
\[`-c`\]

</div>

<div class="cmdsynopsis">

`pg_ctl` `stop` \[`-D` *`datadir`*\] \[`-m` `s[mart]` | `f[ast]` |
`i[mmediate]` \] \[`-W`\] \[`-t` *`seconds`*\] \[`-s`\]

</div>

<div class="cmdsynopsis">

`pg_ctl` `restart` \[`-D` *`datadir`*\] \[`-m` `s[mart]` | `f[ast]` |
`i[mmediate]` \] \[`-W`\] \[`-t` *`seconds`*\] \[`-s`\] \[`-o`
*`options`*\] \[`-c`\]

</div>

<div class="cmdsynopsis">

`pg_ctl` `reload` \[`-D` *`datadir`*\] \[`-s`\]

</div>

<div class="cmdsynopsis">

`pg_ctl` `status` \[`-D` *`datadir`*\]

</div>

<div class="cmdsynopsis">

`pg_ctl` `promote` \[`-D` *`datadir`*\] \[`-W`\] \[`-t` *`seconds`*\]
\[`-s`\]

</div>

<div class="cmdsynopsis">

`pg_ctl` `kill` *`signal_name`* *`process_id`*

</div>

On Microsoft Windows, also:

<div class="cmdsynopsis">

`pg_ctl` `register` \[`-D` *`datadir`*\] \[`-N` *`servicename`*\] \[`-U`
*`username`*\] \[`-P` *`password`*\] \[`-S` `a[uto]` | `d[emand]` \]
\[`-e` *`source`*\] \[`-W`\] \[`-t` *`seconds`*\] \[`-s`\] \[`-o`
*`options`*\]

</div>

<div class="cmdsynopsis">

`pg_ctl` `unregister` \[`-N` *`servicename`*\]

</div>

</div>

<div id="APP-PG-CTL-DESCRIPTION" class="refsect1">

## Description

<span class="application">pg\_ctl</span> is a utility for initializing a
<span class="productname">PostgreSQL</span> database cluster, starting,
stopping, or restarting the <span class="productname">PostgreSQL</span>
database server
([<span class="refentrytitle"><span class="application">postgres</span></span>](app-postgres.html "postgres")),
or displaying the status of a running server. Although the server can be
started manually, <span class="application">pg\_ctl</span> encapsulates
tasks such as redirecting log output and properly detaching from the
terminal and process group. It also provides convenient options for
controlled shutdown.

The `init` or `initdb` mode creates a new
<span class="productname">PostgreSQL</span> database cluster, that is, a
collection of databases that will be managed by a single server
instance. This mode invokes the `initdb` command. See
[<span class="refentrytitle">initdb</span>](app-initdb.html "initdb")
for details.

`start` mode launches a new server. The server is started in the
background, and its standard input is attached to `/dev/null` (or `nul`
on Windows). On Unix-like systems, by default, the server's standard
output and standard error are sent to
<span class="application">pg\_ctl</span>'s standard output (not standard
error). The standard output of <span class="application">pg\_ctl</span>
should then be redirected to a file or piped to another process such as
a log rotating program like <span class="application">rotatelogs</span>;
otherwise `postgres` will write its output to the controlling terminal
(from the background) and will not leave the shell's process group. On
Windows, by default the server's standard output and standard error are
sent to the terminal. These default behaviors can be changed by using
`-l` to append the server's output to a log file. Use of either `-l` or
output redirection is recommended.

`stop` mode shuts down the server that is running in the specified data
directory. Three different shutdown methods can be selected with the
`-m` option.
<span class="quote">“<span class="quote">Smart</span>”</span> mode
waits for all active clients to disconnect and any online backup to
finish. If the server is in hot standby, recovery and streaming
replication will be terminated once all clients have disconnected.
<span class="quote">“<span class="quote">Fast</span>”</span> mode (the
default) does not wait for clients to disconnect and will terminate an
online backup in progress. All active transactions are rolled back and
clients are forcibly disconnected, then the server is shut down.
<span class="quote">“<span class="quote">Immediate</span>”</span> mode
will abort all server processes immediately, without a clean shutdown.
This choice will lead to a crash-recovery cycle during the next server
start.

`restart` mode effectively executes a stop followed by a start. This
allows changing the `postgres` command-line options, or changing
configuration-file options that cannot be changed without restarting the
server. If relative paths were used on the command line during server
start, `restart` might fail unless
<span class="application">pg\_ctl</span> is executed in the same current
directory as it was during server start.

`reload` mode simply sends the `postgres` server process a
<span class="systemitem">SIGHUP</span> signal, causing it to reread its
configuration files (`postgresql.conf`, `pg_hba.conf`, etc.). This
allows changing configuration-file options that do not require a full
server restart to take effect.

`status` mode checks whether a server is running in the specified data
directory. If it is, the server's PID and the command line options that
were used to invoke it are displayed. If the server is not running,
<span class="application">pg\_ctl</span> returns an exit status of 3. If
an accessible data directory is not specified,
<span class="application">pg\_ctl</span> returns an exit status of 4.

`promote` mode commands the standby server that is running in the
specified data directory to end standby mode and begin read-write
operations.

`kill` mode sends a signal to a specified process. This is primarily
valuable on <span class="productname">Microsoft Windows</span> which
does not have a built-in <span class="application">kill</span> command.
Use `--help` to see a list of supported signal names.

`register` mode registers the
<span class="productname">PostgreSQL</span> server as a system service
on <span class="productname">Microsoft Windows</span>. The `-S` option
allows selection of service start type, either
<span class="quote">“<span class="quote">auto</span>”</span> (start
service automatically on system startup) or
<span class="quote">“<span class="quote">demand</span>”</span> (start
service on demand).

`unregister` mode unregisters a system service on
<span class="productname">Microsoft Windows</span>. This undoes the
effects of the `register` command.

</div>

<div id="APP-PG-CTL-OPTIONS" class="refsect1">

## Options

<div class="variablelist">

  - <span class="term">`-c`  
    </span><span class="term">`--core-files`</span>  
    Attempt to allow server crashes to produce core files, on platforms
    where this is possible, by lifting any soft resource limit placed on
    core files. This is useful in debugging or diagnosing problems by
    allowing a stack trace to be obtained from a failed server process.

  - <span class="term">`-D datadir`  
    </span><span class="term">`--pgdata=datadir`</span>  
    Specifies the file system location of the database configuration
    files. If this option is omitted, the environment variable `PGDATA`
    is used.

  - <span class="term">`-l filename`  
    </span><span class="term">`--log=filename`</span>  
    Append the server log output to *`filename`*. If the file does not
    exist, it is created. The <span class="systemitem">umask</span> is
    set to 077, so access to the log file is disallowed to other users
    by default.

  - <span class="term">`-m mode`  
    </span><span class="term">`--mode=mode`</span>  
    Specifies the shutdown mode. *`mode`* can be `smart`, `fast`, or
    `immediate`, or the first letter of one of these three. If this
    option is omitted, `fast` is the default.

  - <span class="term">`-o options`  
    </span><span class="term">`--options=options`</span>  
    Specifies options to be passed directly to the `postgres` command.
    `-o` can be specified multiple times, with all the given options
    being passed through.
    
    The *`options`* should usually be surrounded by single or double
    quotes to ensure that they are passed through as a group.

  - <span class="term">`-o initdb-options`  
    </span><span class="term">`--options=initdb-options`</span>  
    Specifies options to be passed directly to the `initdb` command.
    `-o` can be specified multiple times, with all the given options
    being passed through.
    
    The *`options`* should usually be surrounded by single or double
    quotes to ensure that they are passed through as a group.

  - <span class="term">`-p path`</span>  
    Specifies the location of the `postgres` executable. By default the
    `postgres` executable is taken from the same directory as `pg_ctl`,
    or failing that, the hard-wired installation directory. It is not
    necessary to use this option unless you are doing something unusual
    and get errors that the `postgres` executable was not found.
    
    In `init` mode, this option analogously specifies the location of
    the `initdb` executable.

  - <span class="term">`-s`  
    </span><span class="term">`--silent`</span>  
    Print only errors, no informational messages.

  - <span class="term">`-t seconds`  
    </span><span class="term">`--timeout=seconds`</span>  
    Specifies the maximum number of seconds to wait when waiting for an
    operation to complete (see option `-w`). Defaults to the value of
    the `PGCTLTIMEOUT` environment variable or, if not set, to 60
    seconds.

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">pg\_ctl</span> version and exit.

  - <span class="term">`-w`  
    </span><span class="term">`--wait`</span>  
    Wait for the operation to complete. This is supported for the modes
    `start`, `stop`, `restart`, `promote`, and `register`, and is the
    default for those modes.
    
    When waiting, `pg_ctl` repeatedly checks the server's PID file,
    sleeping for a short amount of time between checks. Startup is
    considered complete when the PID file indicates that the server is
    ready to accept connections. Shutdown is considered complete when
    the server removes the PID file. `pg_ctl` returns an exit code based
    on the success of the startup or shutdown.
    
    If the operation does not complete within the timeout (see option
    `-t`), then `pg_ctl` exits with a nonzero exit status. But note that
    the operation might continue in the background and eventually
    succeed.

  - <span class="term">`-W`  
    </span><span class="term">`--no-wait`</span>  
    Do not wait for the operation to complete. This is the opposite of
    the option `-w`.
    
    If waiting is disabled, the requested action is triggered, but there
    is no feedback about its success. In that case, the server log file
    or an external monitoring system would have to be used to check the
    progress and success of the operation.
    
    In prior releases of PostgreSQL, this was the default except for the
    `stop` mode.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">pg\_ctl</span> command
    line arguments, and exit.

</div>

If an option is specified that is valid, but not relevant to the
selected operating mode, <span class="application">pg\_ctl</span>
ignores it.

<div id="APP-PG-CTL-WINDOWS-OPTIONS" class="refsect2">

### Options for Windows

<div class="variablelist">

  - <span class="term">`-e source`</span>  
    Name of the event source for
    <span class="application">pg\_ctl</span> to use for logging to the
    event log when running as a Windows service. The default is
    `PostgreSQL`. Note that this only controls messages sent from
    <span class="application">pg\_ctl</span> itself; once started, the
    server will use the event source specified by its
    [event\_source](runtime-config-logging.html#GUC-EVENT-SOURCE)
    parameter. Should the server fail very early in startup, before that
    parameter has been set, it might also log using the default event
    source name `PostgreSQL`.

  - <span class="term">`-N servicename`</span>  
    Name of the system service to register. This name will be used as
    both the service name and the display name. The default is
    `PostgreSQL`.

  - <span class="term">`-P password`</span>  
    Password for the user to run the service as.

  - <span class="term">`-S start-type`</span>  
    Start type of the system service. *`start-type`* can be `auto`, or
    `demand`, or the first letter of one of these two. If this option is
    omitted, `auto` is the default.

  - <span class="term">`-U username`</span>  
    User name for the user to run the service as. For domain users, use
    the format `DOMAIN\username`.

</div>

</div>

</div>

<div id="id-1.9.5.6.7" class="refsect1">

## Environment

<div class="variablelist">

  - <span class="term">`PGCTLTIMEOUT`</span>  
    Default limit on the number of seconds to wait when waiting for
    startup or shutdown to complete. If not set, the default is 60
    seconds.

  - <span class="term">`PGDATA`</span>  
    Default data directory location.

</div>

Most `pg_ctl` modes require knowing the data directory location;
therefore, the `-D` option is required unless `PGDATA` is set.

`pg_ctl`, like most other <span class="productname">PostgreSQL</span>
utilities, also uses the environment variables supported by
<span class="application">libpq</span> (see
[Section 33.14](libpq-envars.html "33.14. Environment Variables")).

For additional variables that affect the server, see
[<span class="refentrytitle"><span class="application">postgres</span></span>](app-postgres.html "postgres").

</div>

<div id="id-1.9.5.6.8" class="refsect1">

## Files

<div class="variablelist">

  - <span class="term">`postmaster.pid`</span>  
    <span class="application">pg\_ctl</span> examines this file in the
    data directory to determine whether the server is currently running.

  - <span class="term">`postmaster.opts`</span>  
    If this file exists in the data directory,
    <span class="application">pg\_ctl</span> (in `restart` mode) will
    pass the contents of the file as options to
    <span class="application">postgres</span>, unless overridden by the
    `-o` option. The contents of this file are also displayed in
    `status` mode.

</div>

</div>

<div id="R1-APP-PGCTL-2" class="refsect1">

## Examples

<div id="R2-APP-PGCTL-3" class="refsect2">

### Starting the Server

To start the server, waiting until the server is accepting connections:

``` screen
$ pg_ctl start
```

To start the server using port 5433, and running without `fsync`, use:

``` screen
$ pg_ctl -o "-F -p 5433" start
```

</div>

<div id="R2-APP-PGCTL-4" class="refsect2">

### Stopping the Server

To stop the server, use:

``` screen
$ pg_ctl stop
```

The `-m` option allows control over <span class="emphasis">*how*</span>
the server shuts down:

``` screen
$ pg_ctl stop -m smart
```

</div>

<div id="R2-APP-PGCTL-5" class="refsect2">

### Restarting the Server

Restarting the server is almost equivalent to stopping the server and
starting it again, except that by default, `pg_ctl` saves and reuses the
command line options that were passed to the previously-running
instance. To restart the server using the same options as before, use:

``` screen
$ pg_ctl restart
```

But if `-o` is specified, that replaces any previous options. To restart
using port 5433, disabling `fsync` upon restart:

``` screen
$ pg_ctl -o "-F -p 5433" restart
```

</div>

<div id="R2-APP-PGCTL-6" class="refsect2">

### Showing the Server Status

Here is sample status output from
<span class="application">pg\_ctl</span>:

``` screen
$ pg_ctl status
pg_ctl: server is running (PID: 13718)
/usr/local/pgsql/bin/postgres "-D" "/usr/local/pgsql/data" "-p" "5433" "-B" "128"
```

The second line is the command that would be invoked in restart
mode.

</div>

</div>

<div id="id-1.9.5.6.10" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle">initdb</span>](app-initdb.html "initdb"),
[<span class="refentrytitle"><span class="application">postgres</span></span>](app-postgres.html "postgres")</span>

</div>

</div>

<div class="navfooter">

-----

|                                                  |                             |                                               |
| :----------------------------------------------- | :-------------------------: | --------------------------------------------: |
| [Prev](app-pgcontroldata.html)                   | [Up](reference-server.html) |                   [Next](app-pgresetwal.html) |
| <span class="application">pg\_controldata</span> |     [Home](index.html)      | <span class="application">pg\_resetwal</span> |

</div>
