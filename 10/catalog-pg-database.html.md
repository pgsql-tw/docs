<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.15. `pg_database`

</div>

[Prev](catalog-pg-conversion.html "51.14. pg_conversion") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-db-role-setting.html "51.16. pg_db_role_setting")

-----

<div id="CATALOG-PG-DATABASE" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.15. `pg_database`

</div>

</div>

</div>

<span id="id-1.10.4.17.2" class="indexterm"></span>

The catalog `pg_database` stores information about the available
databases. Databases are created with the
[<span class="refentrytitle">CREATE
DATABASE</span>](sql-createdatabase.html "CREATE DATABASE") command.
Consult
[Chapter 22](managing-databases.html "Chapter 22. Managing Databases")
for details about the meaning of some of the parameters.

Unlike most system catalogs, `pg_database` is shared across all
databases of a cluster: there is only one copy of `pg_database` per
cluster, not one per database.

<div id="id-1.10.4.17.5" class="table">

**Table 51.15. `pg_database`
Columns**

<div class="table-contents">

| Name            | Type        | References          | Description                                                                                                                                                                                                                                                                                                                                                                                      |
| --------------- | ----------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `oid`           | `oid`       |                     | Row identifier (hidden attribute; must be explicitly selected)                                                                                                                                                                                                                                                                                                                                   |
| `datname`       | `name`      |                     | Database name                                                                                                                                                                                                                                                                                                                                                                                    |
| `datdba`        | `oid`       | `pg_authid`.oid     | Owner of the database, usually the user who created it                                                                                                                                                                                                                                                                                                                                           |
| `encoding`      | `int4`      |                     | Character encoding for this database (`pg_encoding_to_char()` can translate this number to the encoding name)                                                                                                                                                                                                                                                                                    |
| `datcollate`    | `name`      |                     | LC\_COLLATE for this database                                                                                                                                                                                                                                                                                                                                                                    |
| `datctype`      | `name`      |                     | LC\_CTYPE for this database                                                                                                                                                                                                                                                                                                                                                                      |
| `datistemplate` | `bool`      |                     | If true, then this database can be cloned by any user with `CREATEDB` privileges; if false, then only superusers or the owner of the database can clone it.                                                                                                                                                                                                                                      |
| `datallowconn`  | `bool`      |                     | If false then no one can connect to this database. This is used to protect the `template0` database from being altered.                                                                                                                                                                                                                                                                          |
| `datconnlimit`  | `int4`      |                     | Sets maximum number of concurrent connections that can be made to this database. -1 means no limit.                                                                                                                                                                                                                                                                                              |
| `datlastsysoid` | `oid`       |                     | Last system OID in the database; useful particularly to <span class="application">pg\_dump</span>                                                                                                                                                                                                                                                                                                |
| `datfrozenxid`  | `xid`       |                     | All transaction IDs before this one have been replaced with a permanent (<span class="quote">“<span class="quote">frozen</span>”</span>) transaction ID in this database. This is used to track whether the database needs to be vacuumed in order to prevent transaction ID wraparound or to allow `pg_xact` to be shrunk. It is the minimum of the per-table `pg_class`.`relfrozenxid` values. |
| `datminmxid`    | `xid`       |                     | All multixact IDs before this one have been replaced with a transaction ID in this database. This is used to track whether the database needs to be vacuumed in order to prevent multixact ID wraparound or to allow `pg_multixact` to be shrunk. It is the minimum of the per-table `pg_class`.`relminmxid` values.                                                                             |
| `dattablespace` | `oid`       | `pg_tablespace`.oid | The default tablespace for the database. Within this database, all tables for which `pg_class`.`reltablespace` is zero will be stored in this tablespace; in particular, all the non-shared system catalogs will be there.                                                                                                                                                                       |
| `datacl`        | `aclitem[]` |                     | Access privileges; see [<span class="refentrytitle">GRANT</span>](sql-grant.html "GRANT") and [<span class="refentrytitle">REVOKE</span>](sql-revoke.html "REVOKE") for details                                                                                                                                                                                                                  |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                    |                     |                                         |
| :--------------------------------- | :-----------------: | --------------------------------------: |
| [Prev](catalog-pg-conversion.html) | [Up](catalogs.html) | [Next](catalog-pg-db-role-setting.html) |
| 51.14. `pg_conversion`             | [Home](index.html)  |             51.16. `pg_db_role_setting` |

</div>
