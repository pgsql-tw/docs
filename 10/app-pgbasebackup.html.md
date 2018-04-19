<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

pg\_basebackup

</div>

[Prev](app-ecpg.html "ecpg") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](pgbench.html "pgbench")

-----

<div id="APP-PGBASEBACKUP" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.9.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle">pg\_basebackup</span>

pg\_basebackup — take a base backup of a
<span class="productname">PostgreSQL</span> cluster

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`pg_basebackup` \[*`option`*...\]

</div>

</div>

<div id="id-1.9.4.9.5" class="refsect1">

## Description

<span class="application">pg\_basebackup</span> is used to take base
backups of a running <span class="productname">PostgreSQL</span>
database cluster. These are taken without affecting other clients to the
database, and can be used both for point-in-time recovery (see
[Section 25.3](continuous-archiving.html "25.3. Continuous Archiving and Point-in-Time Recovery (PITR)"))
and as the starting point for a log shipping or streaming replication
standby servers (see
[Section 26.2](warm-standby.html "26.2. Log-Shipping Standby Servers")).

<span class="application">pg\_basebackup</span> makes a binary copy of
the database cluster files, while making sure the system is put in and
out of backup mode automatically. Backups are always taken of the entire
database cluster; it is not possible to back up individual databases or
database objects. For individual database backups, a tool such as
[<span class="refentrytitle">pg\_dump</span>](app-pgdump.html "pg_dump")
must be used.

The backup is made over a regular
<span class="productname">PostgreSQL</span> connection, and uses the
replication protocol. The connection must be made with a superuser or a
user having `REPLICATION` permissions (see
[Section 21.2](role-attributes.html "21.2. Role Attributes")), and
`pg_hba.conf` must explicitly permit the replication connection. The
server must also be configured with
[max\_wal\_senders](runtime-config-replication.html#GUC-MAX-WAL-SENDERS)
set high enough to leave at least one session available for the backup
and one for WAL streaming (if used).

There can be multiple `pg_basebackup`s running at the same time, but it
is better from a performance point of view to take only one backup, and
copy the result.

<span class="application">pg\_basebackup</span> can make a base backup
from not only the master but also the standby. To take a backup from the
standby, set up the standby so that it can accept replication
connections (that is, set `max_wal_senders` and
[hot\_standby](runtime-config-replication.html#GUC-HOT-STANDBY), and
configure [host-based
authentication](auth-pg-hba-conf.html "20.1. The pg_hba.conf File")).
You will also need to enable
[full\_page\_writes](runtime-config-wal.html#GUC-FULL-PAGE-WRITES) on
the master.

Note that there are some limitations in an online backup from the
standby:

<div class="itemizedlist">

  - The backup history file is not created in the database cluster
    backed up.

  - If you are using `-X none`, there is no guarantee that all WAL files
    required for the backup are archived at the end of backup.

  - If the standby is promoted to the master during online backup, the
    backup fails.

  - All WAL records required for the backup must contain sufficient
    full-page writes, which requires you to enable `full_page_writes` on
    the master and not to use a tool like
    <span class="application">pg\_compresslog</span> as
    `archive_command` to remove full-page writes from WAL files.

</div>

</div>

<div id="id-1.9.4.9.6" class="refsect1">

## Options

The following command-line options control the location and format of
the output.

<div class="variablelist">

  - <span class="term">`-D directory`  
    </span><span class="term">`--pgdata=directory`</span>  
    Directory to write the output to.
    <span class="application">pg\_basebackup</span> will create the
    directory and any parent directories if necessary. The directory may
    already exist, but it is an error if the directory already exists
    and is not empty.
    
    When the backup is in tar mode, and the directory is specified as
    `-` (dash), the tar file will be written to `stdout`.
    
    This option is required.

  - <span class="term">`-F format`  
    </span><span class="term">`--format=format`</span>  
    Selects the format for the output. *`format`* can be one of the
    following:
    
    <div class="variablelist">
    
      - <span class="term">`p`  
        </span><span class="term">`plain`</span>  
        Write the output as plain files, with the same layout as the
        current data directory and tablespaces. When the cluster has no
        additional tablespaces, the whole database will be placed in the
        target directory. If the cluster contains additional
        tablespaces, the main data directory will be placed in the
        target directory, but all other tablespaces will be placed in
        the same absolute path as they have on the server.
        
        This is the default format.
    
      - <span class="term">`t`  
        </span><span class="term">`tar`</span>  
        Write the output as tar files in the target directory. The main
        data directory will be written to a file named `base.tar`, and
        all other tablespaces will be named after the tablespace OID.
        
        If the value `-` (dash) is specified as target directory, the
        tar contents will be written to standard output, suitable for
        piping to for example <span class="productname">gzip</span>.
        This is only possible if the cluster has no additional
        tablespaces and WAL streaming is not used.
    
    </div>

  - <span class="term">`-r rate`  
    </span><span class="term">`--max-rate=rate`</span>  
    The maximum transfer rate of data transferred from the server.
    Values are in kilobytes per second. Use a suffix of `M` to indicate
    megabytes per second. A suffix of `k` is also accepted, and has no
    effect. Valid values are between 32 kilobytes per second and 1024
    megabytes per second.
    
    The purpose is to limit the impact of
    <span class="application">pg\_basebackup</span> on the running
    server.
    
    This option always affects transfer of the data directory. Transfer
    of WAL files is only affected if the collection method is `fetch`.

  - <span class="term">`-R`  
    </span><span class="term">`--write-recovery-conf`</span>  
    Write a minimal `recovery.conf` in the output directory (or into the
    base archive file when using tar format) to ease setting up a
    standby server. The `recovery.conf` file will record the connection
    settings and, if specified, the replication slot that
    <span class="application">pg\_basebackup</span> is using, so that
    the streaming replication will use the same settings later on.

  - <span class="term">`-S slotname`  
    </span><span class="term">`--slot=slotname`</span>  
    This option can only be used together with `-X stream`. It causes
    the WAL streaming to use the specified replication slot. If the base
    backup is intended to be used as a streaming replication standby
    using replication slots, it should then use the same replication
    slot name in `recovery.conf`. That way, it is ensured that the
    server does not remove any necessary WAL data in the time between
    the end of the base backup and the start of streaming replication.
    
    If this option is not specified and the server supports temporary
    replication slots (version 10 and later), then a temporary
    replication slot is automatically used for WAL streaming.

  - <span class="term">`--no-slot`</span>  
    This option prevents the creation of a temporary replication slot
    during the backup even if it's supported by the server.
    
    Temporary replication slots are created by default if no slot name
    is given with the option `-S` when using log streaming.
    
    The main purpose of this option is to allow taking a base backup
    when the server is out of free replication slots. Using replication
    slots is almost always preferred, because it prevents needed WAL
    from being removed by the server during the backup.

  - <span class="term">`-T
    olddir`=*`newdir`*  
    </span><span class="term">`--tablespace-mapping=olddir`=*`newdir`*</span>  
    Relocate the tablespace in directory *`olddir`* to *`newdir`* during
    the backup. To be effective, *`olddir`* must exactly match the path
    specification of the tablespace as it is currently defined. (But it
    is not an error if there is no tablespace in *`olddir`* contained in
    the backup.) Both *`olddir`* and *`newdir`* must be absolute paths.
    If a path happens to contain a `=` sign, escape it with a backslash.
    This option can be specified multiple times for multiple
    tablespaces. See examples below.
    
    If a tablespace is relocated in this way, the symbolic links inside
    the main data directory are updated to point to the new location. So
    the new data directory is ready to be used for a new server instance
    with all tablespaces in the updated locations.

  - <span class="term">`--waldir=waldir`</span>  
    Specifies the location for the write-ahead log directory. *`waldir`*
    must be an absolute path. The write-ahead log directory can only be
    specified when the backup is in plain mode.

  - <span class="term">`-X method`  
    </span><span class="term">`--wal-method=method`</span>  
    Includes the required write-ahead log files (WAL files) in the
    backup. This will include all write-ahead logs generated during the
    backup. Unless the method `none` is specified, it is possible to
    start a postmaster directly in the extracted directory without the
    need to consult the log archive, thus making this a completely
    standalone backup.
    
    The following methods for collecting the write-ahead logs are
    supported:
    
    <div class="variablelist">
    
      - <span class="term">`n`  
        </span><span class="term">`none`</span>  
        Don't include write-ahead log in the backup.
    
      - <span class="term">`f`  
        </span><span class="term">`fetch`</span>  
        The write-ahead log files are collected at the end of the
        backup. Therefore, it is necessary for the
        [wal\_keep\_segments](runtime-config-replication.html#GUC-WAL-KEEP-SEGMENTS)
        parameter to be set high enough that the log is not removed
        before the end of the backup. If the log has been rotated when
        it's time to transfer it, the backup will fail and be unusable.
        
        The write-ahead log files will be written to the `base.tar`
        file.
    
      - <span class="term">`s`  
        </span><span class="term">`stream`</span>  
        Stream the write-ahead log while the backup is created. This
        will open a second connection to the server and start streaming
        the write-ahead log in parallel while running the backup.
        Therefore, it will use up two connections configured by the
        [max\_wal\_senders](runtime-config-replication.html#GUC-MAX-WAL-SENDERS)
        parameter. As long as the client can keep up with write-ahead
        log received, using this mode requires no extra write-ahead logs
        to be saved on the master.
        
        The write-ahead log files are written to a separate file named
        `pg_wal.tar` (if the server is a version earlier than 10, the
        file will be named `pg_xlog.tar`).
        
        This value is the default.
    
    </div>

  - <span class="term">`-z`  
    </span><span class="term">`--gzip`</span>  
    Enables gzip compression of tar file output, with the default
    compression level. Compression is only available when using the tar
    format, and the suffix `.gz` will automatically be added to all tar
    filenames.

  - <span class="term">`-Z level`  
    </span><span class="term">`--compress=level`</span>  
    Enables gzip compression of tar file output, and specifies the
    compression level (0 through 9, 0 being no compression and 9 being
    best compression). Compression is only available when using the tar
    format, and the suffix `.gz` will automatically be added to all tar
    filenames.

</div>

The following command-line options control the generation of the backup
and the running of the program.

<div class="variablelist">

  - <span class="term">`-c fast|spread`  
    </span><span class="term">`--checkpoint=fast|spread`</span>  
    Sets checkpoint mode to fast (immediate) or spread (default) (see
    [Section 25.3.3](continuous-archiving.html#BACKUP-LOWLEVEL-BASE-BACKUP "25.3.3. Making a Base Backup Using the Low Level API")).

  - <span class="term">`-l label`  
    </span><span class="term">`--label=label`</span>  
    Sets the label for the backup. If none is specified, a default value
    of <span class="quote">“<span class="quote">`pg_basebackup base
    backup`</span>”</span> will be used.

  - <span class="term">`-n`  
    </span><span class="term">`--no-clean`</span>  
    By default, when `pg_basebackup` aborts with an error, it removes
    any directories it might have created before discovering that it
    cannot finish the job (for example, data directory and write-ahead
    log directory). This option inhibits tidying-up and is thus useful
    for debugging.
    
    Note that tablespace directories are not cleaned up either way.

  - <span class="term">`-P`  
    </span><span class="term">`--progress`</span>  
    Enables progress reporting. Turning this on will deliver an
    approximate progress report during the backup. Since the database
    may change during the backup, this is only an approximation and may
    not end at exactly `100%`. In particular, when WAL log is included
    in the backup, the total amount of data cannot be estimated in
    advance, and in this case the estimated target size will increase
    once it passes the total estimate without WAL.
    
    When this is enabled, the backup will start by enumerating the size
    of the entire database, and then go back and send the actual
    contents. This may make the backup take slightly longer, and in
    particular it will take longer before the first data is sent.

  - <span class="term">`-N`  
    </span><span class="term">`--no-sync`</span>  
    By default, `pg_basebackup` will wait for all files to be written
    safely to disk. This option causes `pg_basebackup` to return without
    waiting, which is faster, but means that a subsequent operating
    system crash can leave the base backup corrupt. Generally, this
    option is useful for testing but should not be used when creating a
    production installation.

  - <span class="term">`-v`  
    </span><span class="term">`--verbose`</span>  
    Enables verbose mode. Will output some extra steps during startup
    and shutdown, as well as show the exact file name that is currently
    being processed if progress reporting is also enabled.

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
    <span class="application">pg\_basebackup</span> doesn't connect to
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

  - <span class="term">`-s interval`  
    </span><span class="term">`--status-interval=interval`</span>  
    Specifies the number of seconds between status packets sent back to
    the server. This allows for easier monitoring of the progress from
    server. A value of zero disables the periodic status updates
    completely, although an update will still be sent when requested by
    the server, to avoid timeout disconnect. The default value is 10
    seconds.

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
    Force <span class="application">pg\_basebackup</span> to prompt for
    a password before connecting to a database.
    
    This option is never essential, since
    <span class="application">pg\_basebackup</span> will automatically
    prompt for a password if the server demands password authentication.
    However, <span class="application">pg\_basebackup</span> will waste
    a connection attempt finding out that the server wants a password.
    In some cases it is worth typing `-W` to avoid the extra connection
    attempt.

</div>

Other options are also available:

<div class="variablelist">

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Print the <span class="application">pg\_basebackup</span> version
    and exit.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">pg\_basebackup</span>
    command line arguments, and exit.

</div>

</div>

<div id="id-1.9.4.9.7" class="refsect1">

## Environment

This utility, like most other
<span class="productname">PostgreSQL</span> utilities, uses the
environment variables supported by
<span class="application">libpq</span> (see
[Section 33.14](libpq-envars.html "33.14. Environment Variables")).

</div>

<div id="id-1.9.4.9.8" class="refsect1">

## Notes

At the beginning of the backup, a checkpoint needs to be written on the
server the backup is taken from. Especially if the option
`--checkpoint=fast` is not used, this can take some time during which
<span class="application">pg\_basebackup</span> will be appear to be
idle.

The backup will include all files in the data directory and tablespaces,
including the configuration files and any additional files placed in the
directory by third parties, except certain temporary files managed by
PostgreSQL. But only regular files and directories are copied, except
that symbolic links used for tablespaces are preserved. Symbolic links
pointing to certain directories known to PostgreSQL are copied as empty
directories. Other symbolic links and special device files are skipped.
See
[Section 52.4](protocol-replication.html "52.4. Streaming Replication Protocol")
for the precise details.

Tablespaces will in plain format by default be backed up to the same
path they have on the server, unless the option `--tablespace-mapping`
is used. Without this option, running a plain format base backup on the
same host as the server will not work if tablespaces are in use, because
the backup would have to be written to the same directory locations as
the original tablespaces.

When tar format mode is used, it is the user's responsibility to unpack
each tar file before starting the PostgreSQL server. If there are
additional tablespaces, the tar files for them need to be unpacked in
the correct locations. In this case the symbolic links for those
tablespaces will be created by the server according to the contents of
the `tablespace_map` file that is included in the `base.tar` file.

<span class="application">pg\_basebackup</span> works with servers of
the same or an older major version, down to 9.1. However, WAL streaming
mode (`-X stream`) only works with server version 9.3 and later, and tar
format mode (`--format=tar`) of the current version only works with
server version 9.5 or later.

</div>

<div id="id-1.9.4.9.9" class="refsect1">

## Examples

To create a base backup of the server at `mydbserver` and store it in
the local directory `/usr/local/pgsql/data`:

``` screen
$ pg_basebackup -h mydbserver -D /usr/local/pgsql/data
```

To create a backup of the local server with one compressed tar file for
each tablespace, and store it in the directory `backup`, showing a
progress report while running:

``` screen
$ pg_basebackup -D backup -Ft -z -P
```

To create a backup of a single-tablespace local database and compress
this with <span class="productname">bzip2</span>:

``` screen
$ pg_basebackup -D - -Ft -X fetch | bzip2 > backup.tar.bz2
```

(This command will fail if there are multiple tablespaces in the
database.)

To create a backup of a local database where the tablespace in `/opt/ts`
is relocated to
`./backup/ts`:

``` screen
$ pg_basebackup -D backup/data -T /opt/ts=$(pwd)/backup/ts
```

</div>

<div id="id-1.9.4.9.10" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle">pg\_dump</span>](app-pgdump.html "pg_dump")</span>

</div>

</div>

<div class="navfooter">

-----

|                                       |                             |                                          |
| :------------------------------------ | :-------------------------: | ---------------------------------------: |
| [Prev](app-ecpg.html)                 | [Up](reference-client.html) |                     [Next](pgbench.html) |
| <span class="application">ecpg</span> |     [Home](index.html)      | <span class="application">pgbench</span> |

</div>
