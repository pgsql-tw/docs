<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.6. `pg_attrdef`

</div>

[Prev](catalog-pg-amproc.html "51.5. pg_amproc") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-attribute.html "51.7. pg_attribute")

-----

<div id="CATALOG-PG-ATTRDEF" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.6. `pg_attrdef`

</div>

</div>

</div>

<span id="id-1.10.4.8.2" class="indexterm"></span>

The catalog `pg_attrdef` stores column default values. The main
information about columns is stored in `pg_attribute` (see below). Only
columns that explicitly specify a default value (when the table is
created or the column is added) will have an entry here.

<div id="id-1.10.4.8.4" class="table">

**Table 51.6. `pg_attrdef`
Columns**

<div class="table-contents">

| Name      | Type           | References            | Description                                                    |
| --------- | -------------- | --------------------- | -------------------------------------------------------------- |
| `oid`     | `oid`          |                       | Row identifier (hidden attribute; must be explicitly selected) |
| `adrelid` | `oid`          | `pg_class`.oid        | The table this column belongs to                               |
| `adnum`   | `int2`         | `pg_attribute`.attnum | The number of the column                                       |
| `adbin`   | `pg_node_tree` |                       | The internal representation of the column default value        |
| `adsrc`   | `text`         |                       | A human-readable representation of the default value           |

</div>

</div>

  

The `adsrc` field is historical, and is best not used, because it does
not track outside changes that might affect the representation of the
default value. Reverse-compiling the `adbin` field (with `pg_get_expr`
for example) is a better way to display the default
value.

</div>

<div class="navfooter">

-----

|                                |                     |                                   |
| :----------------------------- | :-----------------: | --------------------------------: |
| [Prev](catalog-pg-amproc.html) | [Up](catalogs.html) | [Next](catalog-pg-attribute.html) |
| 51.5. `pg_amproc`              | [Home](index.html)  |              51.7. `pg_attribute` |

</div>
