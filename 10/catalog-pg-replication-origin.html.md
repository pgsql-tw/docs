<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.43. `pg_replication_origin`

</div>

[Prev](catalog-pg-range.html "51.42. pg_range") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-rewrite.html "51.44. pg_rewrite")

-----

<div id="CATALOG-PG-REPLICATION-ORIGIN" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.43. `pg_replication_origin`

</div>

</div>

</div>

<span id="id-1.10.4.45.2" class="indexterm"></span>

The `pg_replication_origin` catalog contains all replication origins
created. For more on replication origins see
[Chapter 49](replication-origins.html "Chapter 49. Replication Progress Tracking").

<div id="id-1.10.4.45.4" class="table">

**Table 51.43. `pg_replication_origin`
Columns**

<div class="table-contents">

| Name      | Type   | References | Description                                                                                  |
| --------- | ------ | ---------- | -------------------------------------------------------------------------------------------- |
| `roident` | `Oid`  |            | A unique, cluster-wide identifier for the replication origin. Should never leave the system. |
| `roname`  | `text` |            | The external, user defined, name of a replication origin.                                    |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                               |                     |                                 |
| :---------------------------- | :-----------------: | ------------------------------: |
| [Prev](catalog-pg-range.html) | [Up](catalogs.html) | [Next](catalog-pg-rewrite.html) |
| 51.42. `pg_range`             | [Home](index.html)  |             51.44. `pg_rewrite` |

</div>
