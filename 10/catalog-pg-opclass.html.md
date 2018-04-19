<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.33. `pg_opclass`

</div>

[Prev](catalog-pg-namespace.html "51.32. pg_namespace") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-operator.html "51.34. pg_operator")

-----

<div id="CATALOG-PG-OPCLASS" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.33. `pg_opclass`

</div>

</div>

</div>

<span id="id-1.10.4.35.2" class="indexterm"></span>

The catalog `pg_opclass` defines index access method operator classes.
Each operator class defines semantics for index columns of a particular
data type and a particular index access method. An operator class
essentially specifies that a particular operator family is applicable to
a particular indexable column data type. The set of operators from the
family that are actually usable with the indexed column are whichever
ones accept the column's data type as their left-hand input.

Operator classes are described at length in
[Section 37.14](xindex.html "37.14. Interfacing Extensions To Indexes").

<div id="id-1.10.4.35.5" class="table">

**Table 51.33. `pg_opclass`
Columns**

<div class="table-contents">

| Name           | Type   | References         | Description                                                    |
| -------------- | ------ | ------------------ | -------------------------------------------------------------- |
| `oid`          | `oid`  |                    | Row identifier (hidden attribute; must be explicitly selected) |
| `opcmethod`    | `oid`  | `pg_am`.oid        | Index access method operator class is for                      |
| `opcname`      | `name` |                    | Name of this operator class                                    |
| `opcnamespace` | `oid`  | `pg_namespace`.oid | Namespace of this operator class                               |
| `opcowner`     | `oid`  | `pg_authid`.oid    | Owner of the operator class                                    |
| `opcfamily`    | `oid`  | `pg_opfamily`.oid  | Operator family containing the operator class                  |
| `opcintype`    | `oid`  | `pg_type`.oid      | Data type that the operator class indexes                      |
| `opcdefault`   | `bool` |                    | True if this operator class is the default for `opcintype`     |
| `opckeytype`   | `oid`  | `pg_type`.oid      | Type of data stored in index, or zero if same as `opcintype`   |

</div>

</div>

  

An operator class's `opcmethod` must match the `opfmethod` of its
containing operator family. Also, there must be no more than one
`pg_opclass` row having `opcdefault` true for any given combination of
`opcmethod` and
`opcintype`.

</div>

<div class="navfooter">

-----

|                                   |                     |                                  |
| :-------------------------------- | :-----------------: | -------------------------------: |
| [Prev](catalog-pg-namespace.html) | [Up](catalogs.html) | [Next](catalog-pg-operator.html) |
| 51.32. `pg_namespace`             | [Home](index.html)  |             51.34. `pg_operator` |

</div>
