<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.23. `pg_foreign_data_wrapper`

</div>

[Prev](catalog-pg-extension.html "51.22. pg_extension") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-foreign-server.html "51.24. pg_foreign_server")

-----

<div id="CATALOG-PG-FOREIGN-DATA-WRAPPER" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.23. `pg_foreign_data_wrapper`

</div>

</div>

</div>

<span id="id-1.10.4.25.2" class="indexterm"></span>

The catalog `pg_foreign_data_wrapper` stores foreign-data wrapper
definitions. A foreign-data wrapper is the mechanism by which external
data, residing on foreign servers, is accessed.

<div id="id-1.10.4.25.4" class="table">

**Table 51.23. `pg_foreign_data_wrapper`
Columns**

<div class="table-contents">

| Name           | Type        | References      | Description                                                                                                                                                                                                                                               |
| -------------- | ----------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `oid`          | `oid`       |                 | Row identifier (hidden attribute; must be explicitly selected)                                                                                                                                                                                            |
| `fdwname`      | `name`      |                 | Name of the foreign-data wrapper                                                                                                                                                                                                                          |
| `fdwowner`     | `oid`       | `pg_authid`.oid | Owner of the foreign-data wrapper                                                                                                                                                                                                                         |
| `fdwhandler`   | `oid`       | `pg_proc`.oid   | References a handler function that is responsible for supplying execution routines for the foreign-data wrapper. Zero if no handler is provided                                                                                                           |
| `fdwvalidator` | `oid`       | `pg_proc`.oid   | References a validator function that is responsible for checking the validity of the options given to the foreign-data wrapper, as well as options for foreign servers and user mappings using the foreign-data wrapper. Zero if no validator is provided |
| `fdwacl`       | `aclitem[]` |                 | Access privileges; see [<span class="refentrytitle">GRANT</span>](sql-grant.html "GRANT") and [<span class="refentrytitle">REVOKE</span>](sql-revoke.html "REVOKE") for details                                                                           |
| `fdwoptions`   | `text[]`    |                 | Foreign-data wrapper specific options, as <span class="quote">“<span class="quote">keyword=value</span>”</span> strings                                                                                                                                   |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                   |                     |                                        |
| :-------------------------------- | :-----------------: | -------------------------------------: |
| [Prev](catalog-pg-extension.html) | [Up](catalogs.html) | [Next](catalog-pg-foreign-server.html) |
| 51.22. `pg_extension`             | [Home](index.html)  |             51.24. `pg_foreign_server` |

</div>
