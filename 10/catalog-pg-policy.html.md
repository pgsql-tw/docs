<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.38. `pg_policy`

</div>

[Prev](catalog-pg-pltemplate.html "51.37. pg_pltemplate") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-proc.html "51.39. pg_proc")

-----

<div id="CATALOG-PG-POLICY" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.38. `pg_policy`

</div>

</div>

</div>

<span id="id-1.10.4.40.2" class="indexterm"></span>

The catalog `pg_policy` stores row level security policies for tables. A
policy includes the kind of command that it applies to (possibly all
commands), the roles that it applies to, the expression to be added as a
security-barrier qualification to queries that include the table, and
the expression to be added as a `WITH CHECK` option for queries that
attempt to add new records to the table.

<div id="id-1.10.4.40.4" class="table">

**Table 51.38. `pg_policy`
Columns**

<div class="table-contents">

| Name            | Type           | References      | Description                                                                                                                             |
| --------------- | -------------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `polname`       | `name`         |                 | The name of the policy                                                                                                                  |
| `polrelid`      | `oid`          | `pg_class`.oid  | The table to which the policy applies                                                                                                   |
| `polcmd`        | `char`         |                 | The command type to which the policy is applied: `r` for `SELECT`, `a` for `INSERT`, `w` for `UPDATE`, `d` for `DELETE`, or `*` for all |
| `polpermissive` | `boolean`      |                 | Is the policy permissive or restrictive?                                                                                                |
| `polroles`      | `oid[]`        | `pg_authid`.oid | The roles to which the policy is applied                                                                                                |
| `polqual`       | `pg_node_tree` |                 | The expression tree to be added to the security barrier qualifications for queries that use the table                                   |
| `polwithcheck`  | `pg_node_tree` |                 | The expression tree to be added to the WITH CHECK qualifications for queries that attempt to add rows to the table                      |

</div>

</div>

  

<div class="note">

### Note

Policies stored in `pg_policy` are applied only when
`pg_class`.`relrowsecurity` is set for their
table.

</div>

</div>

<div class="navfooter">

-----

|                                    |                     |                              |
| :--------------------------------- | :-----------------: | ---------------------------: |
| [Prev](catalog-pg-pltemplate.html) | [Up](catalogs.html) | [Next](catalog-pg-proc.html) |
| 51.37. `pg_pltemplate`             | [Home](index.html)  |             51.39. `pg_proc` |

</div>
