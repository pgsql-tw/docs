<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.4. `pg_amop`

</div>

[Prev](catalog-pg-am.html "51.3. pg_am") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-amproc.html "51.5. pg_amproc")

-----

<div id="CATALOG-PG-AMOP" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.4. `pg_amop`

</div>

</div>

</div>

<span id="id-1.10.4.6.2" class="indexterm"></span>

The catalog `pg_amop` stores information about operators associated with
access method operator families. There is one row for each operator that
is a member of an operator family. A family member can be either a
*search* operator or an *ordering* operator. An operator can appear in
more than one family, but cannot appear in more than one search position
nor more than one ordering position within a family. (It is allowed,
though unlikely, for an operator to be used for both search and ordering
purposes.)

<div id="id-1.10.4.6.4" class="table">

**Table 51.4. `pg_amop`
Columns**

<div class="table-contents">

| Name             | Type   | References        | Description                                                                                                  |
| ---------------- | ------ | ----------------- | ------------------------------------------------------------------------------------------------------------ |
| `oid`            | `oid`  |                   | Row identifier (hidden attribute; must be explicitly selected)                                               |
| `amopfamily`     | `oid`  | `pg_opfamily`.oid | The operator family this entry is for                                                                        |
| `amoplefttype`   | `oid`  | `pg_type`.oid     | Left-hand input data type of operator                                                                        |
| `amoprighttype`  | `oid`  | `pg_type`.oid     | Right-hand input data type of operator                                                                       |
| `amopstrategy`   | `int2` |                   | Operator strategy number                                                                                     |
| `amoppurpose`    | `char` |                   | Operator purpose, either `s` for search or `o` for ordering                                                  |
| `amopopr`        | `oid`  | `pg_operator`.oid | OID of the operator                                                                                          |
| `amopmethod`     | `oid`  | `pg_am`.oid       | Index access method operator family is for                                                                   |
| `amopsortfamily` | `oid`  | `pg_opfamily`.oid | The B-tree operator family this entry sorts according to, if an ordering operator; zero if a search operator |

</div>

</div>

  

A <span class="quote">“<span class="quote">search</span>”</span>
operator entry indicates that an index of this operator family can be
searched to find all rows satisfying `WHERE` *`indexed_column`*
*`operator`* *`constant`*. Obviously, such an operator must return
`boolean`, and its left-hand input type must match the index's column
data type.

An <span class="quote">“<span class="quote">ordering</span>”</span>
operator entry indicates that an index of this operator family can be
scanned to return rows in the order represented by `ORDER BY`
*`indexed_column`* *`operator`* *`constant`*. Such an operator could
return any sortable data type, though again its left-hand input type
must match the index's column data type. The exact semantics of the
`ORDER BY` are specified by the `amopsortfamily` column, which must
reference a B-tree operator family for the operator's result type.

<div class="note">

### Note

At present, it's assumed that the sort order for an ordering operator is
the default for the referenced operator family, i.e., `ASC NULLS LAST`.
This might someday be relaxed by adding additional columns to specify
sort options explicitly.

</div>

An entry's `amopmethod` must match the `opfmethod` of its containing
operator family (including `amopmethod` here is an intentional
denormalization of the catalog structure for performance reasons). Also,
`amoplefttype` and `amoprighttype` must match the `oprleft` and
`oprright` fields of the referenced `pg_operator`
entry.

</div>

<div class="navfooter">

-----

|                            |                     |                                |
| :------------------------- | :-----------------: | -----------------------------: |
| [Prev](catalog-pg-am.html) | [Up](catalogs.html) | [Next](catalog-pg-amproc.html) |
| 51.3. `pg_am`              | [Home](index.html)  |              51.5. `pg_amproc` |

</div>
