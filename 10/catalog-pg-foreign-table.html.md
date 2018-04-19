<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.25. `pg_foreign_table`

</div>

[Prev](catalog-pg-foreign-server.html "51.24. pg_foreign_server") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-index.html "51.26. pg_index")

-----

<div id="CATALOG-PG-FOREIGN-TABLE" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.25. `pg_foreign_table`

</div>

</div>

</div>

<span id="id-1.10.4.27.2" class="indexterm"></span>

The catalog `pg_foreign_table` contains auxiliary information about
foreign tables. A foreign table is primarily represented by a `pg_class`
entry, just like a regular table. Its `pg_foreign_table` entry contains
the information that is pertinent only to foreign tables and not any
other kind of relation.

<div id="id-1.10.4.27.4" class="table">

**Table 51.25. `pg_foreign_table`
Columns**

<div class="table-contents">

| Name        | Type     | References              | Description                                                                                             |
| ----------- | -------- | ----------------------- | ------------------------------------------------------------------------------------------------------- |
| `ftrelid`   | `oid`    | `pg_class`.oid          | OID of the `pg_class` entry for this foreign table                                                      |
| `ftserver`  | `oid`    | `pg_foreign_server`.oid | OID of the foreign server for this foreign table                                                        |
| `ftoptions` | `text[]` |                         | Foreign table options, as <span class="quote">“<span class="quote">keyword=value</span>”</span> strings |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                        |                     |                               |
| :------------------------------------- | :-----------------: | ----------------------------: |
| [Prev](catalog-pg-foreign-server.html) | [Up](catalogs.html) | [Next](catalog-pg-index.html) |
| 51.24. `pg_foreign_server`             | [Home](index.html)  |             51.26. `pg_index` |

</div>
