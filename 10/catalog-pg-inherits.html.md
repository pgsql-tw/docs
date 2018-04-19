<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.27. `pg_inherits`

</div>

[Prev](catalog-pg-index.html "51.26. pg_index") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-init-privs.html "51.28. pg_init_privs")

-----

<div id="CATALOG-PG-INHERITS" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.27. `pg_inherits`

</div>

</div>

</div>

<span id="id-1.10.4.29.2" class="indexterm"></span>

The catalog `pg_inherits` records information about table inheritance
hierarchies. There is one entry for each direct child table in the
database. (Indirect inheritance can be determined by following chains of
entries.)

<div id="id-1.10.4.29.4" class="table">

**Table 51.27. `pg_inherits`
Columns**

<div class="table-contents">

| Name        | Type   | References     | Description                                                                                                                                                                             |
| ----------- | ------ | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `inhrelid`  | `oid`  | `pg_class`.oid | The OID of the child table                                                                                                                                                              |
| `inhparent` | `oid`  | `pg_class`.oid | The OID of the parent table                                                                                                                                                             |
| `inhseqno`  | `int4` |                | If there is more than one direct parent for a child table (multiple inheritance), this number tells the order in which the inherited columns are to be arranged. The count starts at 1. |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                               |                     |                                    |
| :---------------------------- | :-----------------: | ---------------------------------: |
| [Prev](catalog-pg-index.html) | [Up](catalogs.html) | [Next](catalog-pg-init-privs.html) |
| 51.26. `pg_index`             | [Home](index.html)  |             51.28. `pg_init_privs` |

</div>
