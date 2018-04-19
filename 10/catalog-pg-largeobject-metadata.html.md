<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.31. `pg_largeobject_metadata`

</div>

[Prev](catalog-pg-largeobject.html "51.30. pg_largeobject") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-namespace.html "51.32. pg_namespace")

-----

<div id="CATALOG-PG-LARGEOBJECT-METADATA" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.31. `pg_largeobject_metadata`

</div>

</div>

</div>

<span id="id-1.10.4.33.2" class="indexterm"></span>

The catalog `pg_largeobject_metadata` holds metadata associated with
large objects. The actual large object data is stored in
[`pg_largeobject`](catalog-pg-largeobject.html "51.30. pg_largeobject").

<div id="id-1.10.4.33.4" class="table">

**Table 51.31. `pg_largeobject_metadata`
Columns**

<div class="table-contents">

| Name       | Type        | References      | Description                                                                                                                                                                     |
| ---------- | ----------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `oid`      | `oid`       |                 | Row identifier (hidden attribute; must be explicitly selected)                                                                                                                  |
| `lomowner` | `oid`       | `pg_authid`.oid | Owner of the large object                                                                                                                                                       |
| `lomacl`   | `aclitem[]` |                 | Access privileges; see [<span class="refentrytitle">GRANT</span>](sql-grant.html "GRANT") and [<span class="refentrytitle">REVOKE</span>](sql-revoke.html "REVOKE") for details |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                     |                     |                                   |
| :---------------------------------- | :-----------------: | --------------------------------: |
| [Prev](catalog-pg-largeobject.html) | [Up](catalogs.html) | [Next](catalog-pg-namespace.html) |
| 51.30. `pg_largeobject`             | [Home](index.html)  |             51.32. `pg_namespace` |

</div>
