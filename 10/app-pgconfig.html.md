<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

pg\_config

</div>

[Prev](pgbench.html "pgbench") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-pgdump.html "pg_dump")

-----

<div id="APP-PGCONFIG" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.11.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle">pg\_config</span>

pg\_config — retrieve information about the installed version of
<span class="productname">PostgreSQL</span>

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`pg_config` \[*`option`*...\]

</div>

</div>

<div id="id-1.9.4.11.5" class="refsect1">

## Description

The <span class="application">pg\_config</span> utility prints
configuration parameters of the currently installed version of
<span class="productname">PostgreSQL</span>. It is intended, for
example, to be used by software packages that want to interface to
<span class="productname">PostgreSQL</span> to facilitate finding the
required header files and libraries.

</div>

<div id="id-1.9.4.11.6" class="refsect1">

## Options

To use <span class="application">pg\_config</span>, supply one or more
of the following options:

<div class="variablelist">

  - <span class="term">`--bindir`</span>  
    Print the location of user executables. Use this, for example, to
    find the `psql` program. This is normally also the location where
    the `pg_config` program resides.

  - <span class="term">`--docdir`</span>  
    Print the location of documentation files.

  - <span class="term">`--htmldir`</span>  
    Print the location of HTML documentation files.

  - <span class="term">`--includedir`</span>  
    Print the location of C header files of the client interfaces.

  - <span class="term">`--pkgincludedir`</span>  
    Print the location of other C header files.

  - <span class="term">`--includedir-server`</span>  
    Print the location of C header files for server programming.

  - <span class="term">`--libdir`</span>  
    Print the location of object code libraries.

  - <span class="term">`--pkglibdir`</span>  
    Print the location of dynamically loadable modules, or where the
    server would search for them. (Other architecture-dependent data
    files might also be installed in this directory.)

  - <span class="term">`--localedir`</span>  
    Print the location of locale support files. (This will be an empty
    string if locale support was not configured when
    <span class="productname">PostgreSQL</span> was built.)

  - <span class="term">`--mandir`</span>  
    Print the location of manual pages.

  - <span class="term">`--sharedir`</span>  
    Print the location of architecture-independent support files.

  - <span class="term">`--sysconfdir`</span>  
    Print the location of system-wide configuration files.

  - <span class="term">`--pgxs`</span>  
    Print the location of extension makefiles.

  - <span class="term">`--configure`</span>  
    Print the options that were given to the `configure` script when
    <span class="productname">PostgreSQL</span> was configured for
    building. This can be used to reproduce the identical configuration,
    or to find out with what options a binary package was built. (Note
    however that binary packages often contain vendor-specific custom
    patches.) See also the examples below.

  - <span class="term">`--cc`</span>  
    Print the value of the `CC` variable that was used for building
    <span class="productname">PostgreSQL</span>. This shows the C
    compiler used.

  - <span class="term">`--cppflags`</span>  
    Print the value of the `CPPFLAGS` variable that was used for
    building <span class="productname">PostgreSQL</span>. This shows C
    compiler switches needed at preprocessing time (typically, `-I`
    switches).

  - <span class="term">`--cflags`</span>  
    Print the value of the `CFLAGS` variable that was used for building
    <span class="productname">PostgreSQL</span>. This shows C compiler
    switches.

  - <span class="term">`--cflags_sl`</span>  
    Print the value of the `CFLAGS_SL` variable that was used for
    building <span class="productname">PostgreSQL</span>. This shows
    extra C compiler switches used for building shared libraries.

  - <span class="term">`--ldflags`</span>  
    Print the value of the `LDFLAGS` variable that was used for building
    <span class="productname">PostgreSQL</span>. This shows linker
    switches.

  - <span class="term">`--ldflags_ex`</span>  
    Print the value of the `LDFLAGS_EX` variable that was used for
    building <span class="productname">PostgreSQL</span>. This shows
    linker switches used for building executables only.

  - <span class="term">`--ldflags_sl`</span>  
    Print the value of the `LDFLAGS_SL` variable that was used for
    building <span class="productname">PostgreSQL</span>. This shows
    linker switches used for building shared libraries only.

  - <span class="term">`--libs`</span>  
    Print the value of the `LIBS` variable that was used for building
    <span class="productname">PostgreSQL</span>. This normally contains
    `-l` switches for external libraries linked into
    <span class="productname">PostgreSQL</span>.

  - <span class="term">`--version`</span>  
    Print the version of <span class="productname">PostgreSQL</span>.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">pg\_config</span> command
    line arguments, and exit.

</div>

If more than one option is given, the information is printed in that
order, one item per line. If no options are given, all available
information is printed, with labels.

</div>

<div id="id-1.9.4.11.7" class="refsect1">

## Notes

The options `--docdir`, `--pkgincludedir`, `--localedir`, `--mandir`,
`--sharedir`, `--sysconfdir`, `--cc`, `--cppflags`, `--cflags`,
`--cflags_sl`, `--ldflags`, `--ldflags_sl`, and `--libs` were added in
<span class="productname">PostgreSQL</span> 8.1. The option `--htmldir`
was added in <span class="productname">PostgreSQL</span> 8.4. The option
`--ldflags_ex` was added in <span class="productname">PostgreSQL</span>
9.0.

</div>

<div id="id-1.9.4.11.8" class="refsect1">

## Example

To reproduce the build configuration of the current PostgreSQL
installation, run the following command:

``` programlisting
eval ./configure `pg_config --configure`
```

The output of `pg_config --configure` contains shell quotation marks so
arguments with spaces are represented correctly. Therefore, using `eval`
is required for proper
results.

</div>

</div>

<div class="navfooter">

-----

|                                          |                             |                         |
| :--------------------------------------- | :-------------------------: | ----------------------: |
| [Prev](pgbench.html)                     | [Up](reference-client.html) | [Next](app-pgdump.html) |
| <span class="application">pgbench</span> |     [Home](index.html)      |                pg\_dump |

</div>
