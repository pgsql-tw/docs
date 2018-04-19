<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

25.2. File System Level Backup

</div>

[Prev](backup-dump.html "25.1. SQL Dump") 

[Up](backup.html "Chapter 25. Backup and Restore")

Chapter 25. Backup and
Restore

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](continuous-archiving.html "25.3. Continuous Archiving and Point-in-Time Recovery (PITR)")

-----

<div id="BACKUP-FILE" class="sect1">

<div class="titlepage">

<div>

<div>

## 25.2. File System Level Backup

</div>

</div>

</div>

An alternative backup strategy is to directly copy the files that
<span class="productname">PostgreSQL</span> uses to store the data in
the database;
[Section 18.2](creating-cluster.html "18.2. Creating a Database Cluster")
explains where these files are located. You can use whatever method you
prefer for doing file system backups; for example:

``` programlisting
tar -cf backup.tar /usr/local/pgsql/data
```

There are two restrictions, however, which make this method impractical,
or at least inferior to the <span class="application">pg\_dump</span>
method:

<div class="orderedlist">

1.  The database server <span class="emphasis">*must*</span> be shut
    down in order to get a usable backup. Half-way measures such as
    disallowing all connections will <span class="emphasis">*not*</span>
    work (in part because `tar` and similar tools do not take an atomic
    snapshot of the state of the file system, but also because of
    internal buffering within the server). Information about stopping
    the server can be found in
    [Section 18.5](server-shutdown.html "18.5. Shutting Down the Server").
    Needless to say, you also need to shut down the server before
    restoring the data.

2.  If you have dug into the details of the file system layout of the
    database, you might be tempted to try to back up or restore only
    certain individual tables or databases from their respective files
    or directories. This will <span class="emphasis">*not*</span> work
    because the information contained in these files is not usable
    without the commit log files, `pg_xact/*`, which contain the commit
    status of all transactions. A table file is only usable with this
    information. Of course it is also impossible to restore only a table
    and the associated `pg_xact` data because that would render all
    other tables in the database cluster useless. So file system backups
    only work for complete backup and restoration of an entire database
    cluster.

</div>

An alternative file-system backup approach is to make a
<span class="quote">“<span class="quote">consistent
snapshot</span>”</span> of the data directory, if the file system
supports that functionality (and you are willing to trust that it is
implemented correctly). The typical procedure is to make a
<span class="quote">“<span class="quote">frozen snapshot</span>”</span>
of the volume containing the database, then copy the whole data
directory (not just parts, see above) from the snapshot to a backup
device, then release the frozen snapshot. This will work even while the
database server is running. However, a backup created in this way saves
the database files in a state as if the database server was not properly
shut down; therefore, when you start the database server on the
backed-up data, it will think the previous server instance crashed and
will replay the WAL log. This is not a problem; just be aware of it (and
be sure to include the WAL files in your backup). You can perform a
`CHECKPOINT` before taking the snapshot to reduce recovery time.

If your database is spread across multiple file systems, there might not
be any way to obtain exactly-simultaneous frozen snapshots of all the
volumes. For example, if your data files and WAL log are on different
disks, or if tablespaces are on different file systems, it might not be
possible to use snapshot backup because the snapshots
<span class="emphasis">*must*</span> be simultaneous. Read your file
system documentation very carefully before trusting the
consistent-snapshot technique in such situations.

If simultaneous snapshots are not possible, one option is to shut down
the database server long enough to establish all the frozen snapshots.
Another option is to perform a continuous archiving base backup
([Section 25.3.2](continuous-archiving.html#BACKUP-BASE-BACKUP "25.3.2. Making a Base Backup"))
because such backups are immune to file system changes during the
backup. This requires enabling continuous archiving just during the
backup process; restore is done using continuous archive recovery
([Section 25.3.4](continuous-archiving.html#BACKUP-PITR-RECOVERY "25.3.4. Recovering Using a Continuous Archive Backup")).

Another option is to use <span class="application">rsync</span> to
perform a file system backup. This is done by first running
<span class="application">rsync</span> while the database server is
running, then shutting down the database server long enough to do an
`rsync --checksum`. (`--checksum` is necessary because `rsync` only has
file modification-time granularity of one second.) The second
<span class="application">rsync</span> will be quicker than the first,
because it has relatively little data to transfer, and the end result
will be consistent because the server was down. This method allows a
file system backup to be performed with minimal downtime.

Note that a file system backup will typically be larger than an SQL
dump. (<span class="application">pg\_dump</span> does not need to dump
the contents of indexes for example, just the commands to recreate
them.) However, taking a file system backup might be
faster.

</div>

<div class="navfooter">

-----

|                          |                    |                                                              |
| :----------------------- | :----------------: | -----------------------------------------------------------: |
| [Prev](backup-dump.html) | [Up](backup.html)  |                            [Next](continuous-archiving.html) |
| 25.1. SQL Dump           | [Home](index.html) | 25.3. Continuous Archiving and Point-in-Time Recovery (PITR) |

</div>
