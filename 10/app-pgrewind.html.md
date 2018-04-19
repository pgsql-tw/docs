<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">pg\_rewind</span>

</div>

[Prev](app-pgresetwal.html "pg_resetwal") 

[Up](reference-server.html "PostgreSQL Server Applications")

PostgreSQL Server
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](pgtestfsync.html "pg_test_fsync")

-----

<div id="APP-PGREWIND" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.5.8.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">pg\_rewind</span></span>

pg\_rewind — synchronize a <span class="productname">PostgreSQL</span>
data directory with another data directory that was forked from it

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`pg_rewind` \[*`option`*...\] { `-D ` | `--target-pgdata` }*`
directory`* { `--source-pgdata=directory` | `--source-server=connstr` }

</div>

</div>

<div id="id-1.9.5.8.5" class="refsect1">

## Description

<span class="application">pg\_rewind</span> is a tool for synchronizing
a PostgreSQL cluster with another copy of the same cluster, after the
clusters' timelines have diverged. A typical scenario is to bring an old
master server back online after failover as a standby that follows the
new master.

The result is equivalent to replacing the target data directory with the
source one. Only changed blocks from relation files are copied; all
other files are copied in full, including configuration files. The
advantage of <span class="application">pg\_rewind</span> over taking a
new base backup, or tools like <span class="application">rsync</span>,
is that <span class="application">pg\_rewind</span> does not require
reading through unchanged blocks in the cluster. This makes it a lot
faster when the database is large and only a small fraction of blocks
differ between the clusters.

<span class="application">pg\_rewind</span> examines the timeline
histories of the source and target clusters to determine the point where
they diverged, and expects to find WAL in the target cluster's `pg_wal`
directory reaching all the way back to the point of divergence. The
point of divergence can be found either on the target timeline, the
source timeline, or their common ancestor. In the typical failover
scenario where the target cluster was shut down soon after the
divergence, this is not a problem, but if the target cluster ran for a
long time after the divergence, the old WAL files might no longer be
present. In that case, they can be manually copied from the WAL archive
to the `pg_wal` directory, or fetched on startup by configuring
`recovery.conf`. The use of <span class="application">pg\_rewind</span>
is not limited to failover, e.g. a standby server can be promoted, run
some write transactions, and then rewinded to become a standby again.

When the target server is started for the first time after running
<span class="application">pg\_rewind</span>, it will go into recovery
mode and replay all WAL generated in the source server after the point
of divergence. If some of the WAL was no longer available in the source
server when <span class="application">pg\_rewind</span> was run, and
therefore could not be copied by the
<span class="application">pg\_rewind</span> session, it must be made
available when the target server is started. This can be done by
creating a `recovery.conf` file in the target data directory with a
suitable `restore_command`.

<span class="application">pg\_rewind</span> requires that the target
server either has the
[wal\_log\_hints](runtime-config-wal.html#GUC-WAL-LOG-HINTS) option
enabled in `postgresql.conf` or data checksums enabled when the cluster
was initialized with <span class="application">initdb</span>. Neither of
these are currently on by default.
[full\_page\_writes](runtime-config-wal.html#GUC-FULL-PAGE-WRITES) must
also be set to `on`, but is enabled by default.

</div>

<div id="id-1.9.5.8.6" class="refsect1">

## Options

<span class="application">pg\_rewind</span> accepts the following
command-line arguments:

<div class="variablelist">

  - <span class="term">`-D directory`  
    </span><span class="term">`--target-pgdata=directory`</span>  
    This option specifies the target data directory that is synchronized
    with the source. The target server must be shut down cleanly before
    running <span class="application">pg\_rewind</span>

  - <span class="term">`--source-pgdata=directory`</span>  
    Specifies the file system path to the data directory of the source
    server to synchronize the target with. This option requires the
    source server to be cleanly shut down.

  - <span class="term">`--source-server=connstr`</span>  
    Specifies a libpq connection string to connect to the source
    <span class="productname">PostgreSQL</span> server to synchronize
    the target with. The connection must be a normal (non-replication)
    connection with superuser access. This option requires the source
    server to be running and not in recovery mode.

  - <span class="term">`-n`  
    </span><span class="term">`--dry-run`</span>  
    Do everything except actually modifying the target directory.

  - <span class="term">`-P`  
    </span><span class="term">`--progress`</span>  
    Enables progress reporting. Turning this on will deliver an
    approximate progress report while copying data from the source
    cluster.

  - <span class="term">`--debug`</span>  
    Print verbose debugging output that is mostly useful for developers
    debugging <span class="application">pg\_rewind</span>.

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Display version information, then exit.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help, then exit.

</div>

</div>

<div id="id-1.9.5.8.7" class="refsect1">

## Environment

When `--source-server` option is used,
<span class="application">pg\_rewind</span> also uses the environment
variables supported by <span class="application">libpq</span> (see
[Section 33.14](libpq-envars.html "33.14. Environment Variables")).

</div>

<div id="id-1.9.5.8.8" class="refsect1">

## Notes

<div id="id-1.9.5.8.8.2" class="refsect2">

### How it works

The basic idea is to copy all file system-level changes from the source
cluster to the target cluster:

<div class="procedure">

1.  Scan the WAL log of the target cluster, starting from the last
    checkpoint before the point where the source cluster's timeline
    history forked off from the target cluster. For each WAL record,
    record each data block that was touched. This yields a list of all
    the data blocks that were changed in the target cluster, after the
    source cluster forked off.

2.  Copy all those changed blocks from the source cluster to the target
    cluster, either using direct file system access (`--source-pgdata`)
    or SQL (`--source-server`).

3.  Copy all other files such as `pg_xact` and configuration files from
    the source cluster to the target cluster (everything except the
    relation files).

4.  Apply the WAL from the source cluster, starting from the checkpoint
    created at failover. (Strictly speaking,
    <span class="application">pg\_rewind</span> doesn't apply the WAL,
    it just creates a backup label file that makes
    <span class="productname">PostgreSQL</span> start by replaying all
    WAL from that checkpoint
forward.)

</div>

</div>

</div>

</div>

<div class="navfooter">

-----

|                                               |                             |                                                  |
| :-------------------------------------------- | :-------------------------: | -----------------------------------------------: |
| [Prev](app-pgresetwal.html)                   | [Up](reference-server.html) |                         [Next](pgtestfsync.html) |
| <span class="application">pg\_resetwal</span> |     [Home](index.html)      | <span class="application">pg\_test\_fsync</span> |

</div>
