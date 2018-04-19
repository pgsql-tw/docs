<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

F.1. adminpack

</div>

[Prev](contrib.html "Appendix F. Additional Supplied Modules") 

[Up](contrib.html "Appendix F. Additional Supplied Modules")

Appendix F. Additional Supplied Modules

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](amcheck.html "F.2. amcheck")

-----

<div id="ADMINPACK" class="sect1">

<div class="titlepage">

<div>

<div>

## F.1. adminpack

</div>

</div>

</div>

<span id="id-1.11.7.10.2" class="indexterm"></span>

`adminpack` provides a number of support functions which
<span class="application">pgAdmin</span> and other administration and
management tools can use to provide additional functionality, such as
remote management of server log files. Use of all these functions is
restricted to superusers.

The functions shown in
[Table F.1](adminpack.html#FUNCTIONS-ADMINPACK-TABLE "Table F.1. adminpack Functions")
provide write access to files on the machine hosting the server. (See
also the functions in
[Table 9.88](functions-admin.html#FUNCTIONS-ADMIN-GENFILE-TABLE "Table 9.88. Generic File Access Functions"),
which provide read-only access.) Only files within the database cluster
directory can be accessed, but either a relative or absolute path is
allowable.

<div id="FUNCTIONS-ADMINPACK-TABLE" class="table">

**Table F.1. `adminpack`
Functions**

<div class="table-contents">

| Name                                                                         | Return Type    | Description                                         |
| ---------------------------------------------------------------------------- | -------------- | --------------------------------------------------- |
| `pg_catalog.pg_file_write(filename text, data text, append boolean)`         | `bigint`       | Write, or append to, a text file                    |
| `pg_catalog.pg_file_rename(oldname text, newname text [, archivename text])` | `boolean`      | Rename a file                                       |
| `pg_catalog.pg_file_unlink(filename text)`                                   | `boolean`      | Remove a file                                       |
| `pg_catalog.pg_logdir_ls()`                                                  | `setof record` | List the log files in the `log_directory` directory |

</div>

</div>

  
<span id="id-1.11.7.10.6" class="indexterm"></span>

`pg_file_write` writes the specified *`data`* into the file named by
*`filename`*. If *`append`* is false, the file must not already exist.
If *`append`* is true, the file can already exist, and will be appended
to if so. Returns the number of bytes written.

<span id="id-1.11.7.10.8" class="indexterm"></span>

`pg_file_rename` renames a file. If *`archivename`* is omitted or NULL,
it simply renames *`oldname`* to *`newname`* (which must not already
exist). If *`archivename`* is provided, it first renames *`newname`* to
*`archivename`* (which must not already exist), and then renames
*`oldname`* to *`newname`*. In event of failure of the second rename
step, it will try to rename *`archivename`* back to *`newname`* before
reporting the error. Returns true on success, false if the source
file(s) are not present or not writable; other cases throw errors.

<span id="id-1.11.7.10.10" class="indexterm"></span>

`pg_file_unlink` removes the specified file. Returns true on success,
false if the specified file is not present or the `unlink()` call fails;
other cases throw errors.

<span id="id-1.11.7.10.12" class="indexterm"></span>

`pg_logdir_ls` returns the start timestamps and path names of all the
log files in the
[log\_directory](runtime-config-logging.html#GUC-LOG-DIRECTORY)
directory. The
[log\_filename](runtime-config-logging.html#GUC-LOG-FILENAME) parameter
must have its default setting (`postgresql-%Y-%m-%d_%H%M%S.log`) to use
this function.

The functions shown in
[Table F.2](adminpack.html#FUNCTIONS-ADMINPACK-DEPRECATED-TABLE "Table F.2. Deprecated adminpack Functions")
are deprecated and should not be used in new applications; instead use
those shown in
[Table 9.78](functions-admin.html#FUNCTIONS-ADMIN-SIGNAL-TABLE "Table 9.78. Server Signaling Functions")
and
[Table 9.88](functions-admin.html#FUNCTIONS-ADMIN-GENFILE-TABLE "Table 9.88. Generic File Access Functions").
These functions are provided in `adminpack` only for compatibility with
old versions of <span class="application">pgAdmin</span>.

<div id="FUNCTIONS-ADMINPACK-DEPRECATED-TABLE" class="table">

**Table F.2. Deprecated `adminpack`
Functions**

<div class="table-contents">

| Name                                                                   | Return Type | Description                                                                                             |
| ---------------------------------------------------------------------- | ----------- | ------------------------------------------------------------------------------------------------------- |
| `pg_catalog.pg_file_read(filename text, offset bigint, nbytes bigint)` | `text`      | Alternate name for `pg_read_file()`                                                                     |
| `pg_catalog.pg_file_length(filename text)`                             | `bigint`    | Same as `size` column returned by `pg_stat_file()`                                                      |
| `pg_catalog.pg_logfile_rotate()`                                       | `integer`   | Alternate name for `pg_rotate_logfile()`, but note that it returns integer 0 or 1 rather than `boolean` |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                         |                    |                      |
| :-------------------------------------- | :----------------: | -------------------: |
| [Prev](contrib.html)                    | [Up](contrib.html) | [Next](amcheck.html) |
| Appendix F. Additional Supplied Modules | [Home](index.html) |         F.2. amcheck |

</div>
