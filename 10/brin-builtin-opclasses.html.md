<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

65.2. Built-in Operator Classes

</div>

[Prev](brin-intro.html "65.1. Introduction") 

[Up](brin.html "Chapter 65. BRIN Indexes")

Chapter 65. BRIN Indexes

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](brin-extensibility.html "65.3. Extensibility")

-----

<div id="BRIN-BUILTIN-OPCLASSES" class="sect1">

<div class="titlepage">

<div>

<div>

## 65.2. Built-in Operator Classes

</div>

</div>

</div>

The core <span class="productname">PostgreSQL</span> distribution
includes the BRIN operator classes shown in
[Table 65.1](brin-builtin-opclasses.html#BRIN-BUILTIN-OPCLASSES-TABLE "Table 65.1. Built-in BRIN Operator Classes").

The *minmax* operator classes store the minimum and the maximum values
appearing in the indexed column within the range. The *inclusion*
operator classes store a value which includes the values in the indexed
column within the range.

<div id="BRIN-BUILTIN-OPCLASSES-TABLE" class="table">

**Table 65.1. Built-in BRIN Operator
Classes**

<div class="table-contents">

| Name                     | Indexed Data Type             | Indexable Operators                                                 |
| ------------------------ | ----------------------------- | ------------------------------------------------------------------- |
| `abstime_minmax_ops`     | `abstime`                     | `<` `<=` `=` `>=` `>`                                               |
| `int8_minmax_ops`        | `bigint`                      | `<` `<=` `=` `>=` `>`                                               |
| `bit_minmax_ops`         | `bit`                         | `<` `<=` `=` `>=` `>`                                               |
| `varbit_minmax_ops`      | `bit varying`                 | `<` `<=` `=` `>=` `>`                                               |
| `box_inclusion_ops`      | `box`                         | `<<` `&<` `&&` `&>` `>>` `~=` `@>` `<@` `&<\|` `<<\|` `\|>>` `\|&>` |
| `bytea_minmax_ops`       | `bytea`                       | `<` `<=` `=` `>=` `>`                                               |
| `bpchar_minmax_ops`      | `character`                   | `<` `<=` `=` `>=` `>`                                               |
| `char_minmax_ops`        | `"char"`                      | `<` `<=` `=` `>=` `>`                                               |
| `date_minmax_ops`        | `date`                        | `<` `<=` `=` `>=` `>`                                               |
| `float8_minmax_ops`      | `double precision`            | `<` `<=` `=` `>=` `>`                                               |
| `inet_minmax_ops`        | `inet`                        | `<` `<=` `=` `>=` `>`                                               |
| `network_inclusion_ops`  | `inet`                        | `&&` `>>=` `<<=` `=` `>>` `<<`                                      |
| `int4_minmax_ops`        | `integer`                     | `<` `<=` `=` `>=` `>`                                               |
| `interval_minmax_ops`    | `interval`                    | `<` `<=` `=` `>=` `>`                                               |
| `macaddr_minmax_ops`     | `macaddr`                     | `<` `<=` `=` `>=` `>`                                               |
| `macaddr8_minmax_ops`    | `macaddr8`                    | `<` `<=` `=` `>=` `>`                                               |
| `name_minmax_ops`        | `name`                        | `<` `<=` `=` `>=` `>`                                               |
| `numeric_minmax_ops`     | `numeric`                     | `<` `<=` `=` `>=` `>`                                               |
| `pg_lsn_minmax_ops`      | `pg_lsn`                      | `<` `<=` `=` `>=` `>`                                               |
| `oid_minmax_ops`         | `oid`                         | `<` `<=` `=` `>=` `>`                                               |
| `range_inclusion_ops`    | `any range type`              | `<<` `&<` `&&` `&>` `>>` `@>` `<@` `-\|-` `=` `<` `<=` `=` `>` `>=` |
| `float4_minmax_ops`      | `real`                        | `<` `<=` `=` `>=` `>`                                               |
| `reltime_minmax_ops`     | `reltime`                     | `<` `<=` `=` `>=` `>`                                               |
| `int2_minmax_ops`        | `smallint`                    | `<` `<=` `=` `>=` `>`                                               |
| `text_minmax_ops`        | `text`                        | `<` `<=` `=` `>=` `>`                                               |
| `tid_minmax_ops`         | `tid`                         | `<` `<=` `=` `>=` `>`                                               |
| `timestamp_minmax_ops`   | `timestamp without time zone` | `<` `<=` `=` `>=` `>`                                               |
| `timestamptz_minmax_ops` | `timestamp with time zone`    | `<` `<=` `=` `>=` `>`                                               |
| `time_minmax_ops`        | `time without time zone`      | `<` `<=` `=` `>=` `>`                                               |
| `timetz_minmax_ops`      | `time with time zone`         | `<` `<=` `=` `>=` `>`                                               |
| `uuid_minmax_ops`        | `uuid`                        | `<` `<=` `=` `>=` `>`                                               |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                         |                    |                                 |
| :---------------------- | :----------------: | ------------------------------: |
| [Prev](brin-intro.html) |  [Up](brin.html)   | [Next](brin-extensibility.html) |
| 65.1. Introduction      | [Home](index.html) |             65.3. Extensibility |

</div>
