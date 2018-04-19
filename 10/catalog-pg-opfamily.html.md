<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.35. `pg_opfamily`

</div>

[Prev](catalog-pg-operator.html "51.34. pg_operator") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-partitioned-table.html "51.36. pg_partitioned_table")

-----

<div id="CATALOG-PG-OPFAMILY" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.35. `pg_opfamily`

</div>

</div>

</div>

<span id="id-1.10.4.37.2" class="indexterm"></span>

The catalog `pg_opfamily` defines operator families. Each operator
family is a collection of operators and associated support routines that
implement the semantics specified for a particular index access method.
Furthermore, the operators in a family are all
<span class="quote">“<span class="quote">compatible</span>”</span>, in
a way that is specified by the access method. The operator family
concept allows cross-data-type operators to be used with indexes and to
be reasoned about using knowledge of access method semantics.

Operator families are described at length in
[Section 37.14](xindex.html "37.14. Interfacing Extensions To Indexes").

<div id="id-1.10.4.37.5" class="table">

**Table 51.35. `pg_opfamily`
Columns**

<div class="table-contents">

| Name           | Type   | References         | Description                                                    |
| -------------- | ------ | ------------------ | -------------------------------------------------------------- |
| `oid`          | `oid`  |                    | Row identifier (hidden attribute; must be explicitly selected) |
| `opfmethod`    | `oid`  | `pg_am`.oid        | Index access method operator family is for                     |
| `opfname`      | `name` |                    | Name of this operator family                                   |
| `opfnamespace` | `oid`  | `pg_namespace`.oid | Namespace of this operator family                              |
| `opfowner`     | `oid`  | `pg_authid`.oid    | Owner of the operator family                                   |

</div>

</div>

  

The majority of the information defining an operator family is not in
its `pg_opfamily` row, but in the associated rows in
[`pg_amop`](catalog-pg-amop.html "51.4. pg_amop"),
[`pg_amproc`](catalog-pg-amproc.html "51.5. pg_amproc"), and
[`pg_opclass`](catalog-pg-opclass.html "51.33. pg_opclass").

</div>

<div class="navfooter">

-----

|                                  |                     |                                           |
| :------------------------------- | :-----------------: | ----------------------------------------: |
| [Prev](catalog-pg-operator.html) | [Up](catalogs.html) | [Next](catalog-pg-partitioned-table.html) |
| 51.34. `pg_operator`             | [Home](index.html)  |             51.36. `pg_partitioned_table` |

</div>
