<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.9. `pg_auth_members`

</div>

[Prev](catalog-pg-authid.html "51.8. pg_authid") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-cast.html "51.10. pg_cast")

-----

<div id="CATALOG-PG-AUTH-MEMBERS" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.9. `pg_auth_members`

</div>

</div>

</div>

<span id="id-1.10.4.11.2" class="indexterm"></span>

The catalog `pg_auth_members` shows the membership relations between
roles. Any non-circular set of relationships is allowed.

Because user identities are cluster-wide, `pg_auth_members` is shared
across all databases of a cluster: there is only one copy of
`pg_auth_members` per cluster, not one per database.

<div id="id-1.10.4.11.5" class="table">

**Table 51.9. `pg_auth_members`
Columns**

<div class="table-contents">

| Name           | Type   | References      | Description                                                 |
| -------------- | ------ | --------------- | ----------------------------------------------------------- |
| `roleid`       | `oid`  | `pg_authid`.oid | ID of a role that has a member                              |
| `member`       | `oid`  | `pg_authid`.oid | ID of a role that is a member of `roleid`                   |
| `grantor`      | `oid`  | `pg_authid`.oid | ID of the role that granted this membership                 |
| `admin_option` | `bool` |                 | True if `member` can grant membership in `roleid` to others |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                |                     |                              |
| :----------------------------- | :-----------------: | ---------------------------: |
| [Prev](catalog-pg-authid.html) | [Up](catalogs.html) | [Next](catalog-pg-cast.html) |
| 51.8. `pg_authid`              | [Home](index.html)  |             51.10. `pg_cast` |

</div>
