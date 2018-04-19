<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.22. `pg_extension`

</div>

[Prev](catalog-pg-event-trigger.html "51.21. pg_event_trigger") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System
Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-foreign-data-wrapper.html "51.23. pg_foreign_data_wrapper")

-----

<div id="CATALOG-PG-EXTENSION" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.22. `pg_extension`

</div>

</div>

</div>

<span id="id-1.10.4.24.2" class="indexterm"></span>

The catalog `pg_extension` stores information about the installed
extensions. See
[Section 37.15](extend-extensions.html "37.15. Packaging Related Objects into an Extension")
for details about extensions.

<div id="id-1.10.4.24.4" class="table">

**Table 51.22. `pg_extension`
Columns**

<div class="table-contents">

| Name             | Type     | References         | Description                                                                                             |
| ---------------- | -------- | ------------------ | ------------------------------------------------------------------------------------------------------- |
| `oid`            | `oid`    |                    | Row identifier (hidden attribute; must be explicitly selected)                                          |
| `extname`        | `name`   |                    | Name of the extension                                                                                   |
| `extowner`       | `oid`    | `pg_authid`.oid    | Owner of the extension                                                                                  |
| `extnamespace`   | `oid`    | `pg_namespace`.oid | Schema containing the extension's exported objects                                                      |
| `extrelocatable` | `bool`   |                    | True if extension can be relocated to another schema                                                    |
| `extversion`     | `text`   |                    | Version name for the extension                                                                          |
| `extconfig`      | `oid[]`  | `pg_class`.oid     | Array of `regclass` OIDs for the extension's configuration table(s), or `NULL` if none                  |
| `extcondition`   | `text[]` |                    | Array of `WHERE`-clause filter conditions for the extension's configuration table(s), or `NULL` if none |

</div>

</div>

  

Note that unlike most catalogs with a
<span class="quote">“<span class="quote">namespace</span>”</span>
column, `extnamespace` is not meant to imply that the extension belongs
to that schema. Extension names are never schema-qualified. Rather,
`extnamespace` indicates the schema that contains most or all of the
extension's objects. If `extrelocatable` is true, then this schema must
in fact contain all schema-qualifiable objects belonging to the
extension.

</div>

<div class="navfooter">

-----

|                                       |                     |                                              |
| :------------------------------------ | :-----------------: | -------------------------------------------: |
| [Prev](catalog-pg-event-trigger.html) | [Up](catalogs.html) | [Next](catalog-pg-foreign-data-wrapper.html) |
| 51.21. `pg_event_trigger`             | [Home](index.html)  |             51.23. `pg_foreign_data_wrapper` |

</div>
