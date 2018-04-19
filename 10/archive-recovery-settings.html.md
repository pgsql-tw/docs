<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

27.1. Archive Recovery Settings

</div>

[Prev](recovery-config.html "Chapter 27. Recovery Configuration") 

[Up](recovery-config.html "Chapter 27. Recovery Configuration")

Chapter 27. Recovery Configuration

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](recovery-target-settings.html "27.2. Recovery Target Settings")

-----

<div id="ARCHIVE-RECOVERY-SETTINGS" class="sect1">

<div class="titlepage">

<div>

<div>

## 27.1. Archive Recovery Settings

</div>

</div>

</div>

<div class="variablelist">

  - <span class="term">`restore_command` (`string`)
    <span id="id-1.6.14.6.2.1.1.3" class="indexterm"></span> </span>  
    The local shell command to execute to retrieve an archived segment
    of the WAL file series. This parameter is required for archive
    recovery, but optional for streaming replication. Any `%f` in the
    string is replaced by the name of the file to retrieve from the
    archive, and any `%p` is replaced by the copy destination path name
    on the server. (The path name is relative to the current working
    directory, i.e., the cluster's data directory.) Any `%r` is replaced
    by the name of the file containing the last valid restart point.
    That is the earliest file that must be kept to allow a restore to be
    restartable, so this information can be used to truncate the archive
    to just the minimum required to support restarting from the current
    restore. `%r` is typically only used by warm-standby configurations
    (see
    [Section 26.2](warm-standby.html "26.2. Log-Shipping Standby Servers")).
    Write `%%` to embed an actual `%` character.
    
    It is important for the command to return a zero exit status only if
    it succeeds. The command <span class="emphasis">*will*</span> be
    asked for file names that are not present in the archive; it must
    return nonzero when so asked. Examples:
    
    ``` programlisting
    restore_command = 'cp /mnt/server/archivedir/%f "%p"'
    restore_command = 'copy "C:\\server\\archivedir\\%f" "%p"'  # Windows
    ```
    
    An exception is that if the command was terminated by a signal
    (other than <span class="systemitem">SIGTERM</span>, which is used
    as part of a database server shutdown) or an error by the shell
    (such as command not found), then recovery will abort and the server
    will not start up.

  - <span class="term">`archive_cleanup_command` (`string`)
    <span id="id-1.6.14.6.2.2.1.3" class="indexterm"></span> </span>  
    This optional parameter specifies a shell command that will be
    executed at every restartpoint. The purpose of
    `archive_cleanup_command` is to provide a mechanism for cleaning up
    old archived WAL files that are no longer needed by the standby
    server. Any `%r` is replaced by the name of the file containing the
    last valid restart point. That is the earliest file that must be
    <span class="emphasis">*kept*</span> to allow a restore to be
    restartable, and so all files earlier than `%r` may be safely
    removed. This information can be used to truncate the archive to
    just the minimum required to support restart from the current
    restore. The
    [<span class="refentrytitle"><span class="application">pg\_archivecleanup</span></span>](pgarchivecleanup.html "pg_archivecleanup")
    module is often used in `archive_cleanup_command` for single-standby
    configurations, for
    example:
    
    ``` programlisting
    archive_cleanup_command = 'pg_archivecleanup /mnt/server/archivedir %r'
    ```
    
    Note however that if multiple standby servers are restoring from the
    same archive directory, you will need to ensure that you do not
    delete WAL files until they are no longer needed by any of the
    servers. `archive_cleanup_command` would typically be used in a
    warm-standby configuration (see
    [Section 26.2](warm-standby.html "26.2. Log-Shipping Standby Servers")).
    Write `%%` to embed an actual `%` character in the command.
    
    If the command returns a nonzero exit status then a warning log
    message will be written. An exception is that if the command was
    terminated by a signal or an error by the shell (such as command not
    found), a fatal error will be raised.

  - <span class="term">`recovery_end_command` (`string`)
    <span id="id-1.6.14.6.2.3.1.3" class="indexterm"></span> </span>  
    This parameter specifies a shell command that will be executed once
    only at the end of recovery. This parameter is optional. The purpose
    of the `recovery_end_command` is to provide a mechanism for cleanup
    following replication or recovery. Any `%r` is replaced by the name
    of the file containing the last valid restart point, like in
    [archive\_cleanup\_command](archive-recovery-settings.html#ARCHIVE-CLEANUP-COMMAND).
    
    If the command returns a nonzero exit status then a warning log
    message will be written and the database will proceed to start up
    anyway. An exception is that if the command was terminated by a
    signal or an error by the shell (such as command not found), the
    database will not proceed with
startup.

</div>

</div>

<div class="navfooter">

-----

|                                    |                            |                                       |
| :--------------------------------- | :------------------------: | ------------------------------------: |
| [Prev](recovery-config.html)       | [Up](recovery-config.html) | [Next](recovery-target-settings.html) |
| Chapter 27. Recovery Configuration |     [Home](index.html)     |        27.2. Recovery Target Settings |

</div>
