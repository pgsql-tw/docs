<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.40. `pg_publication`

</div>

[Prev](catalog-pg-proc.html "51.39. pg_proc") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-publication-rel.html "51.41. pg_publication_rel")

-----

<div id="CATALOG-PG-PUBLICATION" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.40. `pg_publication`

</div>

</div>

</div>

<span id="id-1.10.4.42.2" class="indexterm"></span>

The catalog `pg_publication` contains all publications created in the
database. For more on publications see
[Section 31.1](logical-replication-publication.html "31.1. Publication").

<div id="id-1.10.4.42.4" class="table">

**Table 51.40. `pg_publication`
Columns**

<div class="table-contents">

| Name           | Type   | References      | Description                                                                                                                    |
| -------------- | ------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `oid`          | `oid`  |                 | Row identifier (hidden attribute; must be explicitly selected)                                                                 |
| `pubname`      | `name` |                 | Name of the publication                                                                                                        |
| `pubowner`     | `oid`  | `pg_authid`.oid | Owner of the publication                                                                                                       |
| `puballtables` | `bool` |                 | If true, this publication automatically includes all tables in the database, including any that will be created in the future. |
| `pubinsert`    | `bool` |                 | If true, `INSERT` operations are replicated for tables in the publication.                                                     |
| `pubupdate`    | `bool` |                 | If true, `UPDATE` operations are replicated for tables in the publication.                                                     |
| `pubdelete`    | `bool` |                 | If true, `DELETE` operations are replicated for tables in the publication.                                                     |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                              |                     |                                         |
| :--------------------------- | :-----------------: | --------------------------------------: |
| [Prev](catalog-pg-proc.html) | [Up](catalogs.html) | [Next](catalog-pg-publication-rel.html) |
| 51.39. `pg_proc`             | [Home](index.html)  |             51.41. `pg_publication_rel` |

</div>
