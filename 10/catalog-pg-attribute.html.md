<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.7. `pg_attribute`

</div>

[Prev](catalog-pg-attrdef.html "51.6. pg_attrdef") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-authid.html "51.8. pg_authid")

-----

<div id="CATALOG-PG-ATTRIBUTE" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.7. `pg_attribute`

</div>

</div>

</div>

<span id="id-1.10.4.9.2" class="indexterm"></span>

The catalog `pg_attribute` stores information about table columns. There
will be exactly one `pg_attribute` row for every column in every table
in the database. (There will also be attribute entries for indexes, and
indeed all objects that have `pg_class` entries.)

The term attribute is equivalent to column and is used for historical
reasons.

<div id="id-1.10.4.9.5" class="table">

**Table 51.7. `pg_attribute`
Columns**

<div class="table-contents">

| Name            | Type        | References         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| --------------- | ----------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `attrelid`      | `oid`       | `pg_class`.oid     | The table this column belongs to                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `attname`       | `name`      |                    | The column name                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `atttypid`      | `oid`       | `pg_type`.oid      | The data type of this column                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `attstattarget` | `int4`      |                    | `attstattarget` controls the level of detail of statistics accumulated for this column by [<span class="refentrytitle">ANALYZE</span>](sql-analyze.html "ANALYZE"). A zero value indicates that no statistics should be collected. A negative value says to use the system default statistics target. The exact meaning of positive values is data type-dependent. For scalar data types, `attstattarget` is both the target number of <span class="quote">“<span class="quote">most common values</span>”</span> to collect, and the target number of histogram bins to create. |
| `attlen`        | `int2`      |                    | A copy of `pg_type.typlen` of this column's type                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `attnum`        | `int2`      |                    | The number of the column. Ordinary columns are numbered from 1 up. System columns, such as `oid`, have (arbitrary) negative numbers.                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `attndims`      | `int4`      |                    | Number of dimensions, if the column is an array type; otherwise 0. (Presently, the number of dimensions of an array is not enforced, so any nonzero value effectively means <span class="quote">“<span class="quote">it's an array</span>”</span>.)                                                                                                                                                                                                                                                                                                                              |
| `attcacheoff`   | `int4`      |                    | Always -1 in storage, but when loaded into a row descriptor in memory this might be updated to cache the offset of the attribute within the row                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `atttypmod`     | `int4`      |                    | `atttypmod` records type-specific data supplied at table creation time (for example, the maximum length of a `varchar` column). It is passed to type-specific input functions and length coercion functions. The value will generally be -1 for types that do not need `atttypmod`.                                                                                                                                                                                                                                                                                              |
| `attbyval`      | `bool`      |                    | A copy of `pg_type.typbyval` of this column's type                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `attstorage`    | `char`      |                    | Normally a copy of `pg_type.typstorage` of this column's type. For TOAST-able data types, this can be altered after column creation to control storage policy.                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `attalign`      | `char`      |                    | A copy of `pg_type.typalign` of this column's type                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `attnotnull`    | `bool`      |                    | This represents a not-null constraint.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `atthasdef`     | `bool`      |                    | This column has a default value, in which case there will be a corresponding entry in the `pg_attrdef` catalog that actually defines the value.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `attidentity`   | `char`      |                    | If a zero byte (`''`), then not an identity column. Otherwise, `a` = generated always, `d` = generated by default.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `attisdropped`  | `bool`      |                    | This column has been dropped and is no longer valid. A dropped column is still physically present in the table, but is ignored by the parser and so cannot be accessed via SQL.                                                                                                                                                                                                                                                                                                                                                                                                  |
| `attislocal`    | `bool`      |                    | This column is defined locally in the relation. Note that a column can be locally defined and inherited simultaneously.                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `attinhcount`   | `int4`      |                    | The number of direct ancestors this column has. A column with a nonzero number of ancestors cannot be dropped nor renamed.                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `attcollation`  | `oid`       | `pg_collation`.oid | The defined collation of the column, or zero if the column is not of a collatable data type.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `attacl`        | `aclitem[]` |                    | Column-level access privileges, if any have been granted specifically on this column                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `attoptions`    | `text[]`    |                    | Attribute-level options, as <span class="quote">“<span class="quote">keyword=value</span>”</span> strings                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `attfdwoptions` | `text[]`    |                    | Attribute-level foreign data wrapper options, as <span class="quote">“<span class="quote">keyword=value</span>”</span> strings                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

</div>

</div>

  

In a dropped column's `pg_attribute` entry, `atttypid` is reset to zero,
but `attlen` and the other fields copied from `pg_type` are still valid.
This arrangement is needed to cope with the situation where the dropped
column's data type was later dropped, and so there is no `pg_type` row
anymore. `attlen` and the other fields can be used to interpret the
contents of a row of the
table.

</div>

<div class="navfooter">

-----

|                                 |                     |                                |
| :------------------------------ | :-----------------: | -----------------------------: |
| [Prev](catalog-pg-attrdef.html) | [Up](catalogs.html) | [Next](catalog-pg-authid.html) |
| 51.6. `pg_attrdef`              | [Home](index.html)  |              51.8. `pg_authid` |

</div>
