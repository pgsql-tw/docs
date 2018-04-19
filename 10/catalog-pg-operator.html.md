<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.34. `pg_operator`

</div>

[Prev](catalog-pg-opclass.html "51.33. pg_opclass") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-opfamily.html "51.35. pg_opfamily")

-----

<div id="CATALOG-PG-OPERATOR" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.34. `pg_operator`

</div>

</div>

</div>

<span id="id-1.10.4.36.2" class="indexterm"></span>

The catalog `pg_operator` stores information about operators. See
[<span class="refentrytitle">CREATE
OPERATOR</span>](sql-createoperator.html "CREATE OPERATOR") and
[Section 37.12](xoper.html "37.12. User-defined Operators") for more
information.

<div id="id-1.10.4.36.4" class="table">

**Table 51.34. `pg_operator`
Columns**

<div class="table-contents">

| Name           | Type      | References         | Description                                                                                                                                                                                                                            |
| -------------- | --------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `oid`          | `oid`     |                    | Row identifier (hidden attribute; must be explicitly selected)                                                                                                                                                                         |
| `oprname`      | `name`    |                    | Name of the operator                                                                                                                                                                                                                   |
| `oprnamespace` | `oid`     | `pg_namespace`.oid | The OID of the namespace that contains this operator                                                                                                                                                                                   |
| `oprowner`     | `oid`     | `pg_authid`.oid    | Owner of the operator                                                                                                                                                                                                                  |
| `oprkind`      | `char`    |                    | `b` = infix (<span class="quote">“<span class="quote">both</span>”</span>), `l` = prefix (<span class="quote">“<span class="quote">left</span>”</span>), `r` = postfix (<span class="quote">“<span class="quote">right</span>”</span>) |
| `oprcanmerge`  | `bool`    |                    | This operator supports merge joins                                                                                                                                                                                                     |
| `oprcanhash`   | `bool`    |                    | This operator supports hash joins                                                                                                                                                                                                      |
| `oprleft`      | `oid`     | `pg_type`.oid      | Type of the left operand                                                                                                                                                                                                               |
| `oprright`     | `oid`     | `pg_type`.oid      | Type of the right operand                                                                                                                                                                                                              |
| `oprresult`    | `oid`     | `pg_type`.oid      | Type of the result                                                                                                                                                                                                                     |
| `oprcom`       | `oid`     | `pg_operator`.oid  | Commutator of this operator, if any                                                                                                                                                                                                    |
| `oprnegate`    | `oid`     | `pg_operator`.oid  | Negator of this operator, if any                                                                                                                                                                                                       |
| `oprcode`      | `regproc` | `pg_proc`.oid      | Function that implements this operator                                                                                                                                                                                                 |
| `oprrest`      | `regproc` | `pg_proc`.oid      | Restriction selectivity estimation function for this operator                                                                                                                                                                          |
| `oprjoin`      | `regproc` | `pg_proc`.oid      | Join selectivity estimation function for this operator                                                                                                                                                                                 |

</div>

</div>

  

Unused column contain zeroes. For example, `oprleft` is zero for a
prefix
operator.

</div>

<div class="navfooter">

-----

|                                 |                     |                                  |
| :------------------------------ | :-----------------: | -------------------------------: |
| [Prev](catalog-pg-opclass.html) | [Up](catalogs.html) | [Next](catalog-pg-opfamily.html) |
| 51.33. `pg_opclass`             | [Home](index.html)  |             51.35. `pg_opfamily` |

</div>
