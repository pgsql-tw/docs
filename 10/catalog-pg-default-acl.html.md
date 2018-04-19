<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.17. `pg_default_acl`

</div>

[Prev](catalog-pg-db-role-setting.html "51.16. pg_db_role_setting") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-depend.html "51.18. pg_depend")

-----

<div id="CATALOG-PG-DEFAULT-ACL" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.17. `pg_default_acl`

</div>

</div>

</div>

<span id="id-1.10.4.19.2" class="indexterm"></span>

The catalog `pg_default_acl` stores initial privileges to be assigned to
newly created objects.

<div id="id-1.10.4.19.4" class="table">

**Table 51.17. `pg_default_acl`
Columns**

<div class="table-contents">

| Name              | Type        | References         | Description                                                                                                |
| ----------------- | ----------- | ------------------ | ---------------------------------------------------------------------------------------------------------- |
| `oid`             | `oid`       |                    | Row identifier (hidden attribute; must be explicitly selected)                                             |
| `defaclrole`      | `oid`       | `pg_authid`.oid    | The OID of the role associated with this entry                                                             |
| `defaclnamespace` | `oid`       | `pg_namespace`.oid | The OID of the namespace associated with this entry, or 0 if none                                          |
| `defaclobjtype`   | `char`      |                    | Type of object this entry is for: `r` = relation (table, view), `S` = sequence, `f` = function, `T` = type |
| `defaclacl`       | `aclitem[]` |                    | Access privileges that this type of object should have on creation                                         |

</div>

</div>

  

A `pg_default_acl` entry shows the initial privileges to be assigned to
an object belonging to the indicated user. There are currently two types
of entry: <span class="quote">“<span class="quote">global</span>”</span>
entries with `defaclnamespace` = 0, and
<span class="quote">“<span class="quote">per-schema</span>”</span>
entries that reference a particular schema. If a global entry is present
then it <span class="emphasis">*overrides*</span> the normal hard-wired
default privileges for the object type. A per-schema entry, if present,
represents privileges to be <span class="emphasis">*added to*</span> the
global or hard-wired default privileges.

Note that when an ACL entry in another catalog is null, it is taken to
represent the hard-wired default privileges for its object,
<span class="emphasis">*not*</span> whatever might be in
`pg_default_acl` at the moment. `pg_default_acl` is only consulted
during object
creation.

</div>

<div class="navfooter">

-----

|                                         |                     |                                |
| :-------------------------------------- | :-----------------: | -----------------------------: |
| [Prev](catalog-pg-db-role-setting.html) | [Up](catalogs.html) | [Next](catalog-pg-depend.html) |
| 51.16. `pg_db_role_setting`             | [Home](index.html)  |             51.18. `pg_depend` |

</div>
