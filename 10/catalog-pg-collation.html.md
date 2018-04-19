<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.12. `pg_collation`

</div>

[Prev](catalog-pg-class.html "51.11. pg_class") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-constraint.html "51.13. pg_constraint")

-----

<div id="CATALOG-PG-COLLATION" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.12. `pg_collation`

</div>

</div>

</div>

<span id="id-1.10.4.14.2" class="indexterm"></span>

The catalog `pg_collation` describes the available collations, which are
essentially mappings from an SQL name to operating system locale
categories. See [Section 23.2](collation.html "23.2. Collation Support")
for more information.

<div id="id-1.10.4.14.4" class="table">

**Table 51.12. `pg_collation`
Columns**

<div class="table-contents">

| Name            | Type   | References         | Description                                                                                                                                                                                                    |
| --------------- | ------ | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `oid`           | `oid`  |                    | Row identifier (hidden attribute; must be explicitly selected)                                                                                                                                                 |
| `collname`      | `name` |                    | Collation name (unique per namespace and encoding)                                                                                                                                                             |
| `collnamespace` | `oid`  | `pg_namespace`.oid | The OID of the namespace that contains this collation                                                                                                                                                          |
| `collowner`     | `oid`  | `pg_authid`.oid    | Owner of the collation                                                                                                                                                                                         |
| `collprovider`  | `char` |                    | Provider of the collation: `d` = database default, `c` = libc, `i` = icu                                                                                                                                       |
| `collencoding`  | `int4` |                    | Encoding in which the collation is applicable, or -1 if it works for any encoding                                                                                                                              |
| `collcollate`   | `name` |                    | `LC_COLLATE` for this collation object                                                                                                                                                                         |
| `collctype`     | `name` |                    | `LC_CTYPE` for this collation object                                                                                                                                                                           |
| `collversion`   | `text` |                    | Provider-specific version of the collation. This is recorded when the collation is created and then checked when it is used, to detect changes in the collation definition that could lead to data corruption. |

</div>

</div>

  

Note that the unique key on this catalog is (`collname`, `collencoding`,
`collnamespace`) not just (`collname`, `collnamespace`).
<span class="productname">PostgreSQL</span> generally ignores all
collations that do not have `collencoding` equal to either the current
database's encoding or -1, and creation of new entries with the same
name as an entry with `collencoding` = -1 is forbidden. Therefore it is
sufficient to use a qualified SQL name (*`schema`*.*`name`*) to identify
a collation, even though this is not unique according to the catalog
definition. The reason for defining the catalog this way is that
<span class="application">initdb</span> fills it in at cluster
initialization time with entries for all locales available on the
system, so it must be able to hold entries for all encodings that might
ever be used in the cluster.

In the `template0` database, it could be useful to create collations
whose encoding does not match the database encoding, since they could
match the encodings of databases later cloned from `template0`. This
would currently have to be done
manually.

</div>

<div class="navfooter">

-----

|                               |                     |                                    |
| :---------------------------- | :-----------------: | ---------------------------------: |
| [Prev](catalog-pg-class.html) | [Up](catalogs.html) | [Next](catalog-pg-constraint.html) |
| 51.11. `pg_class`             | [Home](index.html)  |             51.13. `pg_constraint` |

</div>
