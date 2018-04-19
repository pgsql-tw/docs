<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.14. `pg_conversion`

</div>

[Prev](catalog-pg-constraint.html "51.13. pg_constraint") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-database.html "51.15. pg_database")

-----

<div id="CATALOG-PG-CONVERSION" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.14. `pg_conversion`

</div>

</div>

</div>

<span id="id-1.10.4.16.2" class="indexterm"></span>

The catalog `pg_conversion` describes encoding conversion procedures.
See [<span class="refentrytitle">CREATE
CONVERSION</span>](sql-createconversion.html "CREATE CONVERSION") for
more information.

<div id="id-1.10.4.16.4" class="table">

**Table 51.14. `pg_conversion`
Columns**

<div class="table-contents">

| Name             | Type      | References         | Description                                                    |
| ---------------- | --------- | ------------------ | -------------------------------------------------------------- |
| `oid`            | `oid`     |                    | Row identifier (hidden attribute; must be explicitly selected) |
| `conname`        | `name`    |                    | Conversion name (unique within a namespace)                    |
| `connamespace`   | `oid`     | `pg_namespace`.oid | The OID of the namespace that contains this conversion         |
| `conowner`       | `oid`     | `pg_authid`.oid    | Owner of the conversion                                        |
| `conforencoding` | `int4`    |                    | Source encoding ID                                             |
| `contoencoding`  | `int4`    |                    | Destination encoding ID                                        |
| `conproc`        | `regproc` | `pg_proc`.oid      | Conversion procedure                                           |
| `condefault`     | `bool`    |                    | True if this is the default conversion                         |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                    |                     |                                  |
| :--------------------------------- | :-----------------: | -------------------------------: |
| [Prev](catalog-pg-constraint.html) | [Up](catalogs.html) | [Next](catalog-pg-database.html) |
| 51.13. `pg_constraint`             | [Home](index.html)  |             51.15. `pg_database` |

</div>
