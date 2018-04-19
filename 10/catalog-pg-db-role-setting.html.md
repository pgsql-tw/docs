<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.16. `pg_db_role_setting`

</div>

[Prev](catalog-pg-database.html "51.15. pg_database") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-default-acl.html "51.17. pg_default_acl")

-----

<div id="CATALOG-PG-DB-ROLE-SETTING" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.16. `pg_db_role_setting`

</div>

</div>

</div>

<span id="id-1.10.4.18.2" class="indexterm"></span>

The catalog `pg_db_role_setting` records the default values that have
been set for run-time configuration variables, for each role and
database combination.

Unlike most system catalogs, `pg_db_role_setting` is shared across all
databases of a cluster: there is only one copy of `pg_db_role_setting`
per cluster, not one per database.

<div id="id-1.10.4.18.5" class="table">

**Table 51.16. `pg_db_role_setting`
Columns**

<div class="table-contents">

| Name          | Type     | References        | Description                                                                            |
| ------------- | -------- | ----------------- | -------------------------------------------------------------------------------------- |
| `setdatabase` | `oid`    | `pg_database`.oid | The OID of the database the setting is applicable to, or zero if not database-specific |
| `setrole`     | `oid`    | `pg_authid`.oid   | The OID of the role the setting is applicable to, or zero if not role-specific         |
| `setconfig`   | `text[]` |                   | Defaults for run-time configuration variables                                          |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                  |                     |                                     |
| :------------------------------- | :-----------------: | ----------------------------------: |
| [Prev](catalog-pg-database.html) | [Up](catalogs.html) | [Next](catalog-pg-default-acl.html) |
| 51.15. `pg_database`             | [Home](index.html)  |             51.17. `pg_default_acl` |

</div>
