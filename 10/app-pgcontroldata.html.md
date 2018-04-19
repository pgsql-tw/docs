<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">pg\_controldata</span>

</div>

[Prev](pgarchivecleanup.html "pg_archivecleanup") 

[Up](reference-server.html "PostgreSQL Server Applications")

PostgreSQL Server
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-pg-ctl.html "pg_ctl")

-----

<div id="APP-PGCONTROLDATA" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.5.5.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">pg\_controldata</span></span>

pg\_controldata — display control information of a
<span class="productname">PostgreSQL</span> database cluster

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`pg_controldata` \[*`option`*\] \[\[`-D`\] *`datadir`*\]

</div>

</div>

<div id="R1-APP-PGCONTROLDATA-1" class="refsect1">

## Description

`pg_controldata` prints information initialized during `initdb`, such as
the catalog version. It also shows information about write-ahead logging
and checkpoint processing. This information is cluster-wide, and not
specific to any one database.

This utility can only be run by the user who initialized the cluster
because it requires read access to the data directory. You can specify
the data directory on the command line, or use the environment variable
`PGDATA`. This utility supports the options `-V` and `--version`, which
print the <span class="application">pg\_controldata</span> version and
exit. It also supports options `-?` and `--help`, which output the
supported arguments.

</div>

<div id="id-1.9.5.5.6" class="refsect1">

## Environment

<div class="variablelist">

  - <span class="term">`PGDATA`</span>  
    Default data directory
location

</div>

</div>

</div>

<div class="navfooter">

-----

|                                                     |                             |                                          |
| :-------------------------------------------------- | :-------------------------: | ---------------------------------------: |
| [Prev](pgarchivecleanup.html)                       | [Up](reference-server.html) |                  [Next](app-pg-ctl.html) |
| <span class="application">pg\_archivecleanup</span> |     [Home](index.html)      | <span class="application">pg\_ctl</span> |

</div>
