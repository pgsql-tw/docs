<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.44. `pg_rewrite`

</div>

[Prev](catalog-pg-replication-origin.html "51.43. pg_replication_origin") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-seclabel.html "51.45. pg_seclabel")

-----

<div id="CATALOG-PG-REWRITE" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.44. `pg_rewrite`

</div>

</div>

</div>

<span id="id-1.10.4.46.2" class="indexterm"></span>

The catalog `pg_rewrite` stores rewrite rules for tables and views.

<div id="id-1.10.4.46.4" class="table">

**Table 51.44. `pg_rewrite`
Columns**

<div class="table-contents">

| Name         | Type           | References     | Description                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ------------ | -------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `oid`        | `oid`          |                | Row identifier (hidden attribute; must be explicitly selected)                                                                                                                                                                                                                                                                                                                                                                       |
| `rulename`   | `name`         |                | Rule name                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `ev_class`   | `oid`          | `pg_class`.oid | The table this rule is for                                                                                                                                                                                                                                                                                                                                                                                                           |
| `ev_type`    | `char`         |                | Event type that the rule is for: 1 = `SELECT`, 2 = `UPDATE`, 3 = `INSERT`, 4 = `DELETE`                                                                                                                                                                                                                                                                                                                                              |
| `ev_enabled` | `char`         |                | Controls in which [session\_replication\_role](runtime-config-client.html#GUC-SESSION-REPLICATION-ROLE) modes the rule fires. `O` = rule fires in <span class="quote">“<span class="quote">origin</span>”</span> and <span class="quote">“<span class="quote">local</span>”</span> modes, `D` = rule is disabled, `R` = rule fires in <span class="quote">“<span class="quote">replica</span>”</span> mode, `A` = rule fires always. |
| `is_instead` | `bool`         |                | True if the rule is an `INSTEAD` rule                                                                                                                                                                                                                                                                                                                                                                                                |
| `ev_qual`    | `pg_node_tree` |                | Expression tree (in the form of a `nodeToString()` representation) for the rule's qualifying condition                                                                                                                                                                                                                                                                                                                               |
| `ev_action`  | `pg_node_tree` |                | Query tree (in the form of a `nodeToString()` representation) for the rule's action                                                                                                                                                                                                                                                                                                                                                  |

</div>

</div>

  

<div class="note">

### Note

`pg_class.relhasrules` must be true if a table has any rules in this
catalog.

</div>

</div>

<div class="navfooter">

-----

|                                            |                     |                                  |
| :----------------------------------------- | :-----------------: | -------------------------------: |
| [Prev](catalog-pg-replication-origin.html) | [Up](catalogs.html) | [Next](catalog-pg-seclabel.html) |
| 51.43. `pg_replication_origin`             | [Home](index.html)  |             51.45. `pg_seclabel` |

</div>
