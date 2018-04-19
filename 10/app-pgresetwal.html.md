<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">pg\_resetwal</span>

</div>

[Prev](app-pg-ctl.html "pg_ctl") 

[Up](reference-server.html "PostgreSQL Server Applications")

PostgreSQL Server
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-pgrewind.html "pg_rewind")

-----

<div id="APP-PGRESETWAL" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.5.7.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">pg\_resetwal</span></span>

pg\_resetwal — reset the write-ahead log and other control information
of a <span class="productname">PostgreSQL</span> database cluster

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`pg_resetwal` \[`-f`\] \[`-n`\] \[*`option`*...\] {\[`-D`\] *`datadir`*}

</div>

</div>

<div id="R1-APP-PGRESETWAL-1" class="refsect1">

## Description

`pg_resetwal` clears the write-ahead log (WAL) and optionally resets
some other control information stored in the `pg_control` file. This
function is sometimes needed if these files have become corrupted. It
should be used only as a last resort, when the server will not start due
to such corruption.

After running this command, it should be possible to start the server,
but bear in mind that the database might contain inconsistent data due
to partially-committed transactions. You should immediately dump your
data, run `initdb`, and reload. After reload, check for inconsistencies
and repair as needed.

This utility can only be run by the user who installed the server,
because it requires read/write access to the data directory. For safety
reasons, you must specify the data directory on the command line.
`pg_resetwal` does not use the environment variable `PGDATA`.

If `pg_resetwal` complains that it cannot determine valid data for
`pg_control`, you can force it to proceed anyway by specifying the `-f`
(force) option. In this case plausible values will be substituted for
the missing data. Most of the fields can be expected to match, but
manual assistance might be needed for the next OID, next transaction ID
and epoch, next multitransaction ID and offset, and WAL starting address
fields. These fields can be set using the options discussed below. If
you are not able to determine correct values for all these fields, `-f`
can still be used, but the recovered database must be treated with even
more suspicion than usual: an immediate dump and reload is imperative.
<span class="emphasis">*Do not*</span> execute any data-modifying
operations in the database before you dump, as any such action is likely
to make the corruption worse.

</div>

<div id="id-1.9.5.7.6" class="refsect1">

## Options

<div class="variablelist">

  - <span class="term">`-f`</span>  
    Force `pg_resetwal` to proceed even if it cannot determine valid
    data for `pg_control`, as explained above.

  - <span class="term">`-n`</span>  
    The `-n` (no operation) option instructs `pg_resetwal` to print the
    values reconstructed from `pg_control` and values about to be
    changed, and then exit without modifying anything. This is mainly a
    debugging tool, but can be useful as a sanity check before allowing
    `pg_resetwal` to proceed for real.

  - <span class="term">`-V`  
    </span><span class="term">`--version`</span>  
    Display version information, then exit.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help, then exit.

</div>

The following options are only needed when `pg_resetwal` is unable to
determine appropriate values by reading `pg_control`. Safe values can be
determined as described below. For values that take numeric arguments,
hexadecimal values can be specified by using the prefix `0x`.

<div class="variablelist">

  - <span class="term">`-c` *`xid`*,*`xid`*</span>  
    Manually set the oldest and newest transaction IDs for which the
    commit time can be retrieved.
    
    A safe value for the oldest transaction ID for which the commit time
    can be retrieved (first part) can be determined by looking for the
    numerically smallest file name in the directory `pg_commit_ts` under
    the data directory. Conversely, a safe value for the newest
    transaction ID for which the commit time can be retrieved (second
    part) can be determined by looking for the numerically greatest file
    name in the same directory. The file names are in hexadecimal.

  - <span class="term">`-e` *`xid_epoch`*</span>  
    Manually set the next transaction ID's epoch.
    
    The transaction ID epoch is not actually stored anywhere in the
    database except in the field that is set by `pg_resetwal`, so any
    value will work so far as the database itself is concerned. You
    might need to adjust this value to ensure that replication systems
    such as <span class="application">Slony-I</span> and
    <span class="application">Skytools</span> work correctly — if so, an
    appropriate value should be obtainable from the state of the
    downstream replicated database.

  - <span class="term">`-l` *`walfile`*</span>  
    Manually set the WAL starting address.
    
    The WAL starting address should be larger than any WAL segment file
    name currently existing in the directory `pg_wal` under the data
    directory. These names are also in hexadecimal and have three parts.
    The first part is the
    <span class="quote">“<span class="quote">timeline
    ID</span>”</span> and should usually be kept the same. For
    example, if `00000001000000320000004A` is the largest entry in
    `pg_wal`, use `-l 00000001000000320000004B` or higher.
    
    <div class="note">
    
    ### Note
    
    `pg_resetwal` itself looks at the files in `pg_wal` and chooses a
    default `-l` setting beyond the last existing file name. Therefore,
    manual adjustment of `-l` should only be needed if you are aware of
    WAL segment files that are not currently present in `pg_wal`, such
    as entries in an offline archive; or if the contents of `pg_wal`
    have been lost entirely.
    
    </div>

  - <span class="term">`-m` *`mxid`*,*`mxid`*</span>  
    Manually set the next and oldest multitransaction ID.
    
    A safe value for the next multitransaction ID (first part) can be
    determined by looking for the numerically largest file name in the
    directory `pg_multixact/offsets` under the data directory, adding
    one, and then multiplying by 65536 (0x10000). Conversely, a safe
    value for the oldest multitransaction ID (second part of `-m`) can
    be determined by looking for the numerically smallest file name in
    the same directory and multiplying by 65536. The file names are in
    hexadecimal, so the easiest way to do this is to specify the option
    value in hexadecimal and append four zeroes.

  - <span class="term">`-o` *`oid`*</span>  
    Manually set the next OID.
    
    There is no comparably easy way to determine a next OID that's
    beyond the largest one in the database, but fortunately it is not
    critical to get the next-OID setting right.

  - <span class="term">`-O` *`mxoff`*</span>  
    Manually set the next multitransaction offset.
    
    A safe value can be determined by looking for the numerically
    largest file name in the directory `pg_multixact/members` under the
    data directory, adding one, and then multiplying by 52352 (0xCC80).
    The file names are in hexadecimal. There is no simple recipe such as
    the ones for other options of appending zeroes.

  - <span class="term">`-x` *`xid`*</span>  
    Manually set the next transaction ID.
    
    A safe value can be determined by looking for the numerically
    largest file name in the directory `pg_xact` under the data
    directory, adding one, and then multiplying by 1048576 (0x100000).
    Note that the file names are in hexadecimal. It is usually easiest
    to specify the option value in hexadecimal too. For example, if
    `0011` is the largest entry in `pg_xact`, `-x 0x1200000` will work
    (five trailing zeroes provide the proper multiplier).

</div>

</div>

<div id="id-1.9.5.7.7" class="refsect1">

## Notes

This command must not be used when the server is running. `pg_resetwal`
will refuse to start up if it finds a server lock file in the data
directory. If the server crashed then a lock file might have been left
behind; in that case you can remove the lock file to allow `pg_resetwal`
to run. But before you do so, make doubly certain that there is no
server process still alive.

`pg_resetwal` works only with servers of the same major
version.

</div>

<div id="id-1.9.5.7.8" class="refsect1">

## See Also

<span class="simplelist">[<span class="refentrytitle"><span class="application">pg\_controldata</span></span>](app-pgcontroldata.html "pg_controldata")</span>

</div>

</div>

<div class="navfooter">

-----

|                                          |                             |                                             |
| :--------------------------------------- | :-------------------------: | ------------------------------------------: |
| [Prev](app-pg-ctl.html)                  | [Up](reference-server.html) |                   [Next](app-pgrewind.html) |
| <span class="application">pg\_ctl</span> |     [Home](index.html)      | <span class="application">pg\_rewind</span> |

</div>
