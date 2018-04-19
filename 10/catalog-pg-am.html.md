<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.3. `pg_am`

</div>

[Prev](catalog-pg-aggregate.html "51.2. pg_aggregate") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-amop.html "51.4. pg_amop")

-----

<div id="CATALOG-PG-AM" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.3. `pg_am`

</div>

</div>

</div>

<span id="id-1.10.4.5.2" class="indexterm"></span>

The catalog `pg_am` stores information about relation access methods.
There is one row for each access method supported by the system.
Currently, only indexes have access methods. The requirements for index
access methods are discussed in detail in
[Chapter 60](indexam.html "Chapter 60. Index Access Method Interface Definition").

<div id="id-1.10.4.5.4" class="table">

**Table 51.3. `pg_am`
Columns**

<div class="table-contents">

| Name        | Type      | References    | Description                                                                                     |
| ----------- | --------- | ------------- | ----------------------------------------------------------------------------------------------- |
| `oid`       | `oid`     |               | Row identifier (hidden attribute; must be explicitly selected)                                  |
| `amname`    | `name`    |               | Name of the access method                                                                       |
| `amhandler` | `regproc` | `pg_proc`.oid | OID of a handler function that is responsible for supplying information about the access method |
| `amtype`    | `char`    |               | Currently always `i` to indicate an index access method; other values may be allowed in future  |

</div>

</div>

  

<div class="note">

### Note

Before <span class="productname">PostgreSQL</span> 9.6, `pg_am`
contained many additional columns representing properties of index
access methods. That data is now only directly visible at the C code
level. However, `pg_index_column_has_property()` and related functions
have been added to allow SQL queries to inspect index access method
properties; see
[Table 9.63](functions-info.html#FUNCTIONS-INFO-CATALOG-TABLE "Table 9.63. System Catalog Information Functions").

</div>

</div>

<div class="navfooter">

-----

|                                   |                     |                              |
| :-------------------------------- | :-----------------: | ---------------------------: |
| [Prev](catalog-pg-aggregate.html) | [Up](catalogs.html) | [Next](catalog-pg-amop.html) |
| 51.2. `pg_aggregate`              | [Home](index.html)  |              51.4. `pg_amop` |

</div>
