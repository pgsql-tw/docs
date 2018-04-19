<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.24. `pg_foreign_server`

</div>

[Prev](catalog-pg-foreign-data-wrapper.html "51.23. pg_foreign_data_wrapper") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-foreign-table.html "51.25. pg_foreign_table")

-----

<div id="CATALOG-PG-FOREIGN-SERVER" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.24. `pg_foreign_server`

</div>

</div>

</div>

<span id="id-1.10.4.26.2" class="indexterm"></span>

The catalog `pg_foreign_server` stores foreign server definitions. A
foreign server describes a source of external data, such as a remote
server. Foreign servers are accessed via foreign-data wrappers.

<div id="id-1.10.4.26.4" class="table">

**Table 51.24. `pg_foreign_server`
Columns**

<div class="table-contents">

| Name         | Type        | References                    | Description                                                                                                                                                                     |
| ------------ | ----------- | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `oid`        | `oid`       |                               | Row identifier (hidden attribute; must be explicitly selected)                                                                                                                  |
| `srvname`    | `name`      |                               | Name of the foreign server                                                                                                                                                      |
| `srvowner`   | `oid`       | `pg_authid`.oid               | Owner of the foreign server                                                                                                                                                     |
| `srvfdw`     | `oid`       | `pg_foreign_data_wrapper`.oid | OID of the foreign-data wrapper of this foreign server                                                                                                                          |
| `srvtype`    | `text`      |                               | Type of the server (optional)                                                                                                                                                   |
| `srvversion` | `text`      |                               | Version of the server (optional)                                                                                                                                                |
| `srvacl`     | `aclitem[]` |                               | Access privileges; see [<span class="refentrytitle">GRANT</span>](sql-grant.html "GRANT") and [<span class="refentrytitle">REVOKE</span>](sql-revoke.html "REVOKE") for details |
| `srvoptions` | `text[]`    |                               | Foreign server specific options, as <span class="quote">“<span class="quote">keyword=value</span>”</span> strings                                                               |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                              |                     |                                       |
| :------------------------------------------- | :-----------------: | ------------------------------------: |
| [Prev](catalog-pg-foreign-data-wrapper.html) | [Up](catalogs.html) | [Next](catalog-pg-foreign-table.html) |
| 51.23. `pg_foreign_data_wrapper`             | [Home](index.html)  |             51.25. `pg_foreign_table` |

</div>
