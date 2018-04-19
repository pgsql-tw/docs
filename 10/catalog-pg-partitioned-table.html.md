<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.36. `pg_partitioned_table`

</div>

[Prev](catalog-pg-opfamily.html "51.35. pg_opfamily") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-pltemplate.html "51.37. pg_pltemplate")

-----

<div id="CATALOG-PG-PARTITIONED-TABLE" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.36. `pg_partitioned_table`

</div>

</div>

</div>

<span id="id-1.10.4.38.2" class="indexterm"></span>

The catalog `pg_partitioned_table` stores information about how tables
are partitioned.

<div id="id-1.10.4.38.4" class="table">

**Table 51.36. `pg_partitioned_table`
Columns**

<div class="table-contents">

| Name            | Type           | References            | Description                                                                                                                                                                                                                                                                                                                                                |
| --------------- | -------------- | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `partrelid`     | `oid`          | `pg_class`.oid        | The OID of the `pg_class` entry for this partitioned table                                                                                                                                                                                                                                                                                                 |
| `partstrat`     | `char`         |                       | Partitioning strategy; `l` = list partitioned table, `r` = range partitioned table                                                                                                                                                                                                                                                                         |
| `partnatts`     | `int2`         |                       | The number of columns in partition key                                                                                                                                                                                                                                                                                                                     |
| `partattrs`     | `int2vector`   | `pg_attribute`.attnum | This is an array of `partnatts` values that indicate which table columns are part of the partition key. For example, a value of `1 3` would mean that the first and the third table columns make up the partition key. A zero in this array indicates that the corresponding partition key column is an expression, rather than a simple column reference. |
| `partclass`     | `oidvector`    | `pg_opclass`.oid      | For each column in the partition key, this contains the OID of the operator class to use. See [`pg_opclass`](catalog-pg-opclass.html "51.33. pg_opclass") for details.                                                                                                                                                                                     |
| `partcollation` | `oidvector`    | `pg_opclass`.oid      | For each column in the partition key, this contains the OID of the collation to use for partitioning, or zero if the column is not of a collatable data type.                                                                                                                                                                                              |
| `partexprs`     | `pg_node_tree` |                       | Expression trees (in `nodeToString()` representation) for partition key columns that are not simple column references. This is a list with one element for each zero entry in `partattrs`. Null if all partition key columns are simple references.                                                                                                        |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                  |                     |                                    |
| :------------------------------- | :-----------------: | ---------------------------------: |
| [Prev](catalog-pg-opfamily.html) | [Up](catalogs.html) | [Next](catalog-pg-pltemplate.html) |
| 51.35. `pg_opfamily`             | [Home](index.html)  |             51.37. `pg_pltemplate` |

</div>
