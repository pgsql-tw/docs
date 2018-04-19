<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.29. `pg_language`

</div>

[Prev](catalog-pg-init-privs.html "51.28. pg_init_privs") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-largeobject.html "51.30. pg_largeobject")

-----

<div id="CATALOG-PG-LANGUAGE" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.29. `pg_language`

</div>

</div>

</div>

<span id="id-1.10.4.31.2" class="indexterm"></span>

The catalog `pg_language` registers languages in which you can write
functions or stored procedures. See [<span class="refentrytitle">CREATE
LANGUAGE</span>](sql-createlanguage.html "CREATE LANGUAGE") and
[Chapter 41](xplang.html "Chapter 41. Procedural Languages") for more
information about language handlers.

<div id="id-1.10.4.31.4" class="table">

**Table 51.29. `pg_language`
Columns**

<div class="table-contents">

| Name            | Type        | References      | Description                                                                                                                                                                                                                                                                     |
| --------------- | ----------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `oid`           | `oid`       |                 | Row identifier (hidden attribute; must be explicitly selected)                                                                                                                                                                                                                  |
| `lanname`       | `name`      |                 | Name of the language                                                                                                                                                                                                                                                            |
| `lanowner`      | `oid`       | `pg_authid`.oid | Owner of the language                                                                                                                                                                                                                                                           |
| `lanispl`       | `bool`      |                 | This is false for internal languages (such as SQL) and true for user-defined languages. Currently, <span class="application">pg\_dump</span> still uses this to determine which languages need to be dumped, but this might be replaced by a different mechanism in the future. |
| `lanpltrusted`  | `bool`      |                 | True if this is a trusted language, which means that it is believed not to grant access to anything outside the normal SQL execution environment. Only superusers can create functions in untrusted languages.                                                                  |
| `lanplcallfoid` | `oid`       | `pg_proc`.oid   | For noninternal languages this references the language handler, which is a special function that is responsible for executing all functions that are written in the particular language                                                                                         |
| `laninline`     | `oid`       | `pg_proc`.oid   | This references a function that is responsible for executing <span class="quote">“<span class="quote">inline</span>”</span> anonymous code blocks ([<span class="refentrytitle">DO</span>](sql-do.html "DO") blocks). Zero if inline blocks are not supported.                  |
| `lanvalidator`  | `oid`       | `pg_proc`.oid   | This references a language validator function that is responsible for checking the syntax and validity of new functions when they are created. Zero if no validator is provided.                                                                                                |
| `lanacl`        | `aclitem[]` |                 | Access privileges; see [<span class="refentrytitle">GRANT</span>](sql-grant.html "GRANT") and [<span class="refentrytitle">REVOKE</span>](sql-revoke.html "REVOKE") for details                                                                                                 |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                    |                     |                                     |
| :--------------------------------- | :-----------------: | ----------------------------------: |
| [Prev](catalog-pg-init-privs.html) | [Up](catalogs.html) | [Next](catalog-pg-largeobject.html) |
| 51.28. `pg_init_privs`             | [Home](index.html)  |             51.30. `pg_largeobject` |

</div>
