<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.41. `pg_publication_rel`

</div>

[Prev](catalog-pg-publication.html "51.40. pg_publication") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-range.html "51.42. pg_range")

-----

<div id="CATALOG-PG-PUBLICATION-REL" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.41. `pg_publication_rel`

</div>

</div>

</div>

<span id="id-1.10.4.43.2" class="indexterm"></span>

The catalog `pg_publication_rel` contains the mapping between relations
and publications in the database. This is a many-to-many mapping. See
also
[Section 51.78](view-pg-publication-tables.html "51.78. pg_publication_tables")
for a more user-friendly view of this information.

<div id="id-1.10.4.43.4" class="table">

**Table 51.41. `pg_publication_rel` Columns**

<div class="table-contents">

| Name      | Type  | References           | Description              |
| --------- | ----- | -------------------- | ------------------------ |
| `prpubid` | `oid` | `pg_publication`.oid | Reference to publication |
| `prrelid` | `oid` | `pg_class`.oid       | Reference to relation    |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                     |                     |                               |
| :---------------------------------- | :-----------------: | ----------------------------: |
| [Prev](catalog-pg-publication.html) | [Up](catalogs.html) | [Next](catalog-pg-range.html) |
| 51.40. `pg_publication`             | [Home](index.html)  |             51.42. `pg_range` |

</div>
