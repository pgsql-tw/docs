<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.42. `pg_range`

</div>

[Prev](catalog-pg-publication-rel.html "51.41. pg_publication_rel") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System
Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-replication-origin.html "51.43. pg_replication_origin")

-----

<div id="CATALOG-PG-RANGE" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.42. `pg_range`

</div>

</div>

</div>

<span id="id-1.10.4.44.2" class="indexterm"></span>

The catalog `pg_range` stores information about range types. This is in
addition to the types' entries in
[`pg_type`](catalog-pg-type.html "51.62. pg_type").

<div id="id-1.10.4.44.4" class="table">

**Table 51.42. `pg_range`
Columns**

<div class="table-contents">

| Name           | Type      | References         | Description                                                                                                 |
| -------------- | --------- | ------------------ | ----------------------------------------------------------------------------------------------------------- |
| `rngtypid`     | `oid`     | `pg_type`.oid      | OID of the range type                                                                                       |
| `rngsubtype`   | `oid`     | `pg_type`.oid      | OID of the element type (subtype) of this range type                                                        |
| `rngcollation` | `oid`     | `pg_collation`.oid | OID of the collation used for range comparisons, or 0 if none                                               |
| `rngsubopc`    | `oid`     | `pg_opclass`.oid   | OID of the subtype's operator class used for range comparisons                                              |
| `rngcanonical` | `regproc` | `pg_proc`.oid      | OID of the function to convert a range value into canonical form, or 0 if none                              |
| `rngsubdiff`   | `regproc` | `pg_proc`.oid      | OID of the function to return the difference between two element values as `double precision`, or 0 if none |

</div>

</div>

  

`rngsubopc` (plus `rngcollation`, if the element type is collatable)
determines the sort ordering used by the range type. `rngcanonical` is
used when the element type is discrete. `rngsubdiff` is optional but
should be supplied to improve performance of GiST indexes on the range
type.

</div>

<div class="navfooter">

-----

|                                         |                     |                                            |
| :-------------------------------------- | :-----------------: | -----------------------------------------: |
| [Prev](catalog-pg-publication-rel.html) | [Up](catalogs.html) | [Next](catalog-pg-replication-origin.html) |
| 51.41. `pg_publication_rel`             | [Home](index.html)  |             51.43. `pg_replication_origin` |

</div>
