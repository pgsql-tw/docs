<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

67.2. BKI Commands

</div>

[Prev](bki-format.html "67.1. BKI File Format") 

[Up](bki.html "Chapter 67. BKI Backend Interface")

Chapter 67. BKI Backend Interface

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](bki-structure.html "67.3. Structure of the Bootstrap BKI File")

-----

<div id="BKI-COMMANDS" class="sect1">

<div class="titlepage">

<div>

<div>

## 67.2. BKI Commands

</div>

</div>

</div>

<div class="variablelist">

  - <span class="term"> `create` *`tablename`* *`tableoid`*
    \[<span class="optional">`bootstrap`</span>\]
    \[<span class="optional">`shared_relation`</span>\]
    \[<span class="optional">`without_oids`</span>\]
    \[<span class="optional">`rowtype_oid` *`oid`*</span>\] (*`name1`* =
    *`type1`* \[<span class="optional">FORCE NOT NULL | FORCE NULL
    </span>\] \[<span class="optional">, *`name2`* = *`type2`*
    \[<span class="optional">FORCE NOT NULL | FORCE NULL </span>\],
    ...</span>\]) </span>  
    Create a table named *`tablename`*, and having the OID *`tableoid`*,
    with the columns given in parentheses.
    
    The following column types are supported directly by `bootstrap.c`:
    `bool`, `bytea`, `char` (1 byte), `name`, `int2`, `int4`, `regproc`,
    `regclass`, `regtype`, `text`, `oid`, `tid`, `xid`, `cid`,
    `int2vector`, `oidvector`, `_int4` (array), `_text` (array), `_oid`
    (array), `_char` (array), `_aclitem` (array). Although it is
    possible to create tables containing columns of other types, this
    cannot be done until after `pg_type` has been created and filled
    with appropriate entries. (That effectively means that only these
    column types can be used in bootstrapped tables, but non-bootstrap
    catalogs can contain any built-in type.)
    
    When `bootstrap` is specified, the table will only be created on
    disk; nothing is entered into `pg_class`, `pg_attribute`, etc, for
    it. Thus the table will not be accessible by ordinary SQL operations
    until such entries are made the hard way (with `insert` commands).
    This option is used for creating `pg_class` etc themselves.
    
    The table is created as shared if `shared_relation` is specified. It
    will have OIDs unless `without_oids` is specified. The table's row
    type OID (`pg_type` OID) can optionally be specified via the
    `rowtype_oid` clause; if not specified, an OID is automatically
    generated for it. (The `rowtype_oid` clause is useless if
    `bootstrap` is specified, but it can be provided anyway for
    documentation.)

  - <span class="term"> `open` *`tablename`* </span>  
    Open the table named *`tablename`* for insertion of data. Any
    currently open table is closed.

  - <span class="term"> `close`
    \[<span class="optional">*`tablename`*</span>\] </span>  
    Close the open table. The name of the table can be given as a
    cross-check, but this is not required.

  - <span class="term"> `insert` \[<span class="optional">`OID =`
    *`oid_value`*</span>\] `(` *`value1`* *`value2`* ... `)` </span>  
    Insert a new row into the open table using *`value1`*, *`value2`*,
    etc., for its column values and *`oid_value`* for its OID. If
    *`oid_value`* is zero (0) or the clause is omitted, and the table
    has OIDs, then the next available OID is assigned.
    
    NULL values can be specified using the special key word `_null_`.
    Values containing spaces must be double quoted.

  - <span class="term"> `declare`
    \[<span class="optional">`unique`</span>\] `index` *`indexname`*
    *`indexoid`* `on` *`tablename`* `using` *`amname`* `(` *`opclass1`*
    *`name1`* \[<span class="optional">, ...</span>\] `)` </span>  
    Create an index named *`indexname`*, having OID *`indexoid`*, on the
    table named *`tablename`*, using the *`amname`* access method. The
    fields to index are called *`name1`*, *`name2`* etc., and the
    operator classes to use are *`opclass1`*, *`opclass2`* etc.,
    respectively. The index file is created and appropriate catalog
    entries are made for it, but the index contents are not initialized
    by this command.

  - <span class="term"> `declare toast` *`toasttableoid`*
    *`toastindexoid`* `on` *`tablename`* </span>  
    Create a TOAST table for the table named *`tablename`*. The TOAST
    table is assigned OID *`toasttableoid`* and its index is assigned
    OID *`toastindexoid`*. As with `declare index`, filling of the index
    is postponed.

  - <span class="term">`build indices`</span>  
    Fill in the indices that have previously been
declared.

</div>

</div>

<div class="navfooter">

-----

|                         |                    |                                           |
| :---------------------- | :----------------: | ----------------------------------------: |
| [Prev](bki-format.html) |   [Up](bki.html)   |                [Next](bki-structure.html) |
| 67.1. BKI File Format   | [Home](index.html) | 67.3. Structure of the Bootstrap BKI File |

</div>
