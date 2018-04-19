<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

<span class="application" data-xmlns="http://www.w3.org/1999/xhtml">ecpg</span>

</div>

[Prev](app-dropuser.html "dropuser") 

[Up](reference-client.html "PostgreSQL Client Applications")

PostgreSQL Client
Applications

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](app-pgbasebackup.html "pg_basebackup")

-----

<div id="APP-ECPG" class="refentry">

<div class="titlepage">

</div>

<span id="id-1.9.4.8.1" class="indexterm"></span>

<div class="refnamediv">

## <span class="refentrytitle"><span class="application">ecpg</span></span>

<span class="application">ecpg</span> — embedded SQL C preprocessor

</div>

<div class="refsynopsisdiv">

## Synopsis

<div class="cmdsynopsis">

`ecpg` \[*`option`*...\] *`file`*...

</div>

</div>

<div id="APP-ECPG-DESCRIPTION" class="refsect1">

## Description

`ecpg` is the embedded SQL preprocessor for C programs. It converts C
programs with embedded SQL statements to normal C code by replacing the
SQL invocations with special function calls. The output files can then
be processed with any C compiler tool chain.

`ecpg` will convert each input file given on the command line to the
corresponding C output file. Input files preferably have the extension
`.pgc`. The extension will be replaced by `.c` to determine the output
file name. The output file name can also be overridden using the `-o`
option.

This reference page does not describe the embedded SQL language. See
[Chapter 35](ecpg.html "Chapter 35. ECPG - Embedded SQL in C") for more
information on that topic.

</div>

<div id="id-1.9.4.8.6" class="refsect1">

## Options

`ecpg` accepts the following command-line arguments:

<div class="variablelist">

  - <span class="term">`-c`</span>  
    Automatically generate certain C code from SQL code. Currently, this
    works for `EXEC SQL TYPE`.

  - <span class="term">`-C mode`</span>  
    Set a compatibility mode. *`mode`* can be `INFORMIX` or
    `INFORMIX_SE`.

  - <span class="term">`-D symbol`</span>  
    Define a C preprocessor symbol.

  - <span class="term">`-i`</span>  
    Parse system include files as well.

  - <span class="term">`-I directory`</span>  
    Specify an additional include path, used to find files included via
    `EXEC SQL INCLUDE`. Defaults are `.` (current directory),
    `/usr/local/include`, the
    <span class="productname">PostgreSQL</span> include directory which
    is defined at compile time (default: `/usr/local/pgsql/include`),
    and `/usr/include`, in that order.

  - <span class="term">`-o filename`</span>  
    Specifies that `ecpg` should write all its output to the given
    *`filename`*.

  - <span class="term">`-r option`</span>  
    Selects run-time behavior. *`Option`* can be one of the following:
    
    <div class="variablelist">
    
      - <span class="term">`no_indicator`</span>  
        Do not use indicators but instead use special values to
        represent null values. Historically there have been databases
        using this approach.
    
      - <span class="term">`prepare`</span>  
        Prepare all statements before using them. Libecpg will keep a
        cache of prepared statements and reuse a statement if it gets
        executed again. If the cache runs full, libecpg will free the
        least used statement.
    
      - <span class="term">`questionmarks`</span>  
        Allow question mark as placeholder for compatibility reasons.
        This used to be the default long ago.
    
    </div>

  - <span class="term">`-t`</span>  
    Turn on autocommit of transactions. In this mode, each SQL command
    is automatically committed unless it is inside an explicit
    transaction block. In the default mode, commands are committed only
    when `EXEC SQL COMMIT` is issued.

  - <span class="term">`-v`</span>  
    Print additional information including the version and the "include"
    path.

  - <span class="term">`--version`</span>  
    Print the <span class="application">ecpg</span> version and exit.

  - <span class="term">`-?`  
    </span><span class="term">`--help`</span>  
    Show help about <span class="application">ecpg</span> command line
    arguments, and exit.

</div>

</div>

<div id="id-1.9.4.8.7" class="refsect1">

## Notes

When compiling the preprocessed C code files, the compiler needs to be
able to find the <span class="application">ECPG</span> header files in
the <span class="productname">PostgreSQL</span> include directory.
Therefore, you might have to use the `-I` option when invoking the
compiler (e.g., `-I/usr/local/pgsql/include`).

Programs using C code with embedded SQL have to be linked against the
`libecpg` library, for example using the linker options
`-L/usr/local/pgsql/lib -lecpg`.

The value of either of these directories that is appropriate for the
installation can be found out using
[<span class="refentrytitle">pg\_config</span>](app-pgconfig.html "pg_config").

</div>

<div id="id-1.9.4.8.8" class="refsect1">

## Examples

If you have an embedded SQL C source file named `prog1.pgc`, you can
create an executable program using the following sequence of commands:

``` programlisting
ecpg prog1.pgc
cc -I/usr/local/pgsql/include -c prog1.c
cc -o prog1 prog1.o -L/usr/local/pgsql/lib -lecpg
```

</div>

</div>

<div class="navfooter">

-----

|                                           |                             |                               |
| :---------------------------------------- | :-------------------------: | ----------------------------: |
| [Prev](app-dropuser.html)                 | [Up](reference-client.html) | [Next](app-pgbasebackup.html) |
| <span class="application">dropuser</span> |     [Home](index.html)      |                pg\_basebackup |

</div>
