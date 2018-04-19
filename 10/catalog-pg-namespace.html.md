<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.32. `pg_namespace`

</div>

[Prev](catalog-pg-largeobject-metadata.html "51.31. pg_largeobject_metadata") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-opclass.html "51.33. pg_opclass")

-----

<div id="CATALOG-PG-NAMESPACE" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.32. `pg_namespace`

</div>

</div>

</div>

<span id="id-1.10.4.34.2" class="indexterm"></span>

The catalog `pg_namespace` stores namespaces. A namespace is the
structure underlying SQL schemas: each namespace can have a separate
collection of relations, types, etc. without name conflicts.

<div id="id-1.10.4.34.4" class="table">

**Table 51.32. `pg_namespace`
Columns**

<div class="table-contents">

| Name       | Type        | References      | Description                                                                                                                                                                     |
| ---------- | ----------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `oid`      | `oid`       |                 | Row identifier (hidden attribute; must be explicitly selected)                                                                                                                  |
| `nspname`  | `name`      |                 | Name of the namespace                                                                                                                                                           |
| `nspowner` | `oid`       | `pg_authid`.oid | Owner of the namespace                                                                                                                                                          |
| `nspacl`   | `aclitem[]` |                 | Access privileges; see [<span class="refentrytitle">GRANT</span>](sql-grant.html "GRANT") and [<span class="refentrytitle">REVOKE</span>](sql-revoke.html "REVOKE") for details |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                              |                     |                                 |
| :------------------------------------------- | :-----------------: | ------------------------------: |
| [Prev](catalog-pg-largeobject-metadata.html) | [Up](catalogs.html) | [Next](catalog-pg-opclass.html) |
| 51.31. `pg_largeobject_metadata`             | [Home](index.html)  |             51.33. `pg_opclass` |

</div>
