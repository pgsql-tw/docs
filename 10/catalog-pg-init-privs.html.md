<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.28. `pg_init_privs`

</div>

[Prev](catalog-pg-inherits.html "51.27. pg_inherits") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-language.html "51.29. pg_language")

-----

<div id="CATALOG-PG-INIT-PRIVS" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.28. `pg_init_privs`

</div>

</div>

</div>

<span id="id-1.10.4.30.2" class="indexterm"></span>

The catalog `pg_init_privs` records information about the initial
privileges of objects in the system. There is one entry for each object
in the database which has a non-default (non-NULL) initial set of
privileges.

Objects can have initial privileges either by having those privileges
set when the system is initialized (by
<span class="application">initdb</span>) or when the object is created
during a `CREATE EXTENSION` and the extension script sets initial
privileges using the `GRANT` system. Note that the system will
automatically handle recording of the privileges during the extension
script and that extension authors need only use the `GRANT` and `REVOKE`
statements in their script to have the privileges recorded. The
`privtype` column indicates if the initial privilege was set by
<span class="application">initdb</span> or during a `CREATE EXTENSION`
command.

Objects which have initial privileges set by
<span class="application">initdb</span> will have entries where
`privtype` is `'i'`, while objects which have initial privileges set by
`CREATE EXTENSION` will have entries where `privtype` is `'e'`.

<div id="id-1.10.4.30.6" class="table">

**Table 51.28. `pg_init_privs`
Columns**

<div class="table-contents">

| Name        | Type        | References     | Description                                                                                                                                                                                 |
| ----------- | ----------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `objoid`    | `oid`       | any OID column | The OID of the specific object                                                                                                                                                              |
| `classoid`  | `oid`       | `pg_class`.oid | The OID of the system catalog the object is in                                                                                                                                              |
| `objsubid`  | `int4`      |                | For a table column, this is the column number (the `objoid` and `classoid` refer to the table itself). For all other object types, this column is zero.                                     |
| `privtype`  | `char`      |                | A code defining the type of initial privilege of this object; see text                                                                                                                      |
| `initprivs` | `aclitem[]` |                | The initial access privileges; see [<span class="refentrytitle">GRANT</span>](sql-grant.html "GRANT") and [<span class="refentrytitle">REVOKE</span>](sql-revoke.html "REVOKE") for details |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                  |                     |                                  |
| :------------------------------- | :-----------------: | -------------------------------: |
| [Prev](catalog-pg-inherits.html) | [Up](catalogs.html) | [Next](catalog-pg-language.html) |
| 51.27. `pg_inherits`             | [Home](index.html)  |             51.29. `pg_language` |

</div>
