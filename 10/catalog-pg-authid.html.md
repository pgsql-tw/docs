<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.8. `pg_authid`

</div>

[Prev](catalog-pg-attribute.html "51.7. pg_attribute") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-auth-members.html "51.9. pg_auth_members")

-----

<div id="CATALOG-PG-AUTHID" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.8. `pg_authid`

</div>

</div>

</div>

<span id="id-1.10.4.10.2" class="indexterm"></span>

The catalog `pg_authid` contains information about database
authorization identifiers (roles). A role subsumes the concepts of
<span class="quote">“<span class="quote">users</span>”</span> and
<span class="quote">“<span class="quote">groups</span>”</span>. A user
is essentially just a role with the `rolcanlogin` flag set. Any role
(with or without `rolcanlogin`) can have other roles as members; see
[`pg_auth_members`](catalog-pg-auth-members.html "51.9. pg_auth_members").

Since this catalog contains passwords, it must not be publicly readable.
[`pg_roles`](view-pg-roles.html "51.81. pg_roles") is a publicly
readable view on `pg_authid` that blanks out the password field.

[Chapter 21](user-manag.html "Chapter 21. Database Roles") contains
detailed information about user and privilege management.

Because user identities are cluster-wide, `pg_authid` is shared across
all databases of a cluster: there is only one copy of `pg_authid` per
cluster, not one per database.

<div id="id-1.10.4.10.7" class="table">

**Table 51.8. `pg_authid`
Columns**

<div class="table-contents">

| Name             | Type          | Description                                                                                                                               |
| ---------------- | ------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `oid`            | `oid`         | Row identifier (hidden attribute; must be explicitly selected)                                                                            |
| `rolname`        | `name`        | Role name                                                                                                                                 |
| `rolsuper`       | `bool`        | Role has superuser privileges                                                                                                             |
| `rolinherit`     | `bool`        | Role automatically inherits privileges of roles it is a member of                                                                         |
| `rolcreaterole`  | `bool`        | Role can create more roles                                                                                                                |
| `rolcreatedb`    | `bool`        | Role can create databases                                                                                                                 |
| `rolcanlogin`    | `bool`        | Role can log in. That is, this role can be given as the initial session authorization identifier                                          |
| `rolreplication` | `bool`        | Role is a replication role. A replication role can initiate replication connections and create and drop replication slots.                |
| `rolbypassrls`   | `bool`        | Role bypasses every row level security policy, see [Section 5.7](ddl-rowsecurity.html "5.7. Row Security Policies") for more information. |
| `rolconnlimit`   | `int4`        | For roles that can log in, this sets maximum number of concurrent connections this role can make. -1 means no limit.                      |
| `rolpassword`    | `text`        | Password (possibly encrypted); null if none. The format depends on the form of encryption used.                                           |
| `rolvaliduntil`  | `timestamptz` | Password expiry time (only used for password authentication); null if no expiration                                                       |

</div>

</div>

  

For an MD5 encrypted password, `rolpassword` column will begin with the
string `md5` followed by a 32-character hexadecimal MD5 hash. The MD5
hash will be of the user's password concatenated to their user name. For
example, if user `joe` has password `xyzzy`,
<span class="productname">PostgreSQL</span> will store the md5 hash of
`xyzzyjoe`.

If the password is encrypted with SCRAM-SHA-256, it has the format:

``` synopsis
SCRAM-SHA-256$<iteration count>:<salt>$<StoredKey>:<ServerKey>
```

where *`salt`*, *`StoredKey`* and *`ServerKey`* are in Base64 encoded
format. This format is the same as that specified by RFC 5803.

A password that does not follow either of those formats is assumed to be
unencrypted.

</div>

<div class="navfooter">

-----

|                                   |                     |                                      |
| :-------------------------------- | :-----------------: | -----------------------------------: |
| [Prev](catalog-pg-attribute.html) | [Up](catalogs.html) | [Next](catalog-pg-auth-members.html) |
| 51.7. `pg_attribute`              | [Home](index.html)  |              51.9. `pg_auth_members` |

</div>
