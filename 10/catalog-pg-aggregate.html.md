<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.2. `pg_aggregate`

</div>

[Prev](catalogs-overview.html "51.1. Overview") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-am.html "51.3. pg_am")

-----

<div id="CATALOG-PG-AGGREGATE" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.2. `pg_aggregate`

</div>

</div>

</div>

<span id="id-1.10.4.4.2" class="indexterm"></span>

The catalog `pg_aggregate` stores information about aggregate functions.
An aggregate function is a function that operates on a set of values
(typically one column from each row that matches a query condition) and
returns a single value computed from all these values. Typical aggregate
functions are `sum`, `count`, and `max`. Each entry in `pg_aggregate` is
an extension of an entry in `pg_proc`. The `pg_proc` entry carries the
aggregate's name, input and output data types, and other information
that is similar to ordinary functions.

<div id="id-1.10.4.4.4" class="table">

**Table 51.2. `pg_aggregate`
Columns**

<div class="table-contents">

| Name               | Type      | References        | Description                                                                                                                                                                                                                                                                                                                        |
| ------------------ | --------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `aggfnoid`         | `regproc` | `pg_proc`.oid     | `pg_proc` OID of the aggregate function                                                                                                                                                                                                                                                                                            |
| `aggkind`          | `char`    |                   | Aggregate kind: `n` for <span class="quote">“<span class="quote">normal</span>”</span> aggregates, `o` for <span class="quote">“<span class="quote">ordered-set</span>”</span> aggregates, or `h` for <span class="quote">“<span class="quote">hypothetical-set</span>”</span> aggregates                                          |
| `aggnumdirectargs` | `int2`    |                   | Number of direct (non-aggregated) arguments of an ordered-set or hypothetical-set aggregate, counting a variadic array as one argument. If equal to `pronargs`, the aggregate must be variadic and the variadic array describes the aggregated arguments as well as the final direct arguments. Always zero for normal aggregates. |
| `aggtransfn`       | `regproc` | `pg_proc`.oid     | Transition function                                                                                                                                                                                                                                                                                                                |
| `aggfinalfn`       | `regproc` | `pg_proc`.oid     | Final function (zero if none)                                                                                                                                                                                                                                                                                                      |
| `aggcombinefn`     | `regproc` | `pg_proc`.oid     | Combine function (zero if none)                                                                                                                                                                                                                                                                                                    |
| `aggserialfn`      | `regproc` | `pg_proc`.oid     | Serialization function (zero if none)                                                                                                                                                                                                                                                                                              |
| `aggdeserialfn`    | `regproc` | `pg_proc`.oid     | Deserialization function (zero if none)                                                                                                                                                                                                                                                                                            |
| `aggmtransfn`      | `regproc` | `pg_proc`.oid     | Forward transition function for moving-aggregate mode (zero if none)                                                                                                                                                                                                                                                               |
| `aggminvtransfn`   | `regproc` | `pg_proc`.oid     | Inverse transition function for moving-aggregate mode (zero if none)                                                                                                                                                                                                                                                               |
| `aggmfinalfn`      | `regproc` | `pg_proc`.oid     | Final function for moving-aggregate mode (zero if none)                                                                                                                                                                                                                                                                            |
| `aggfinalextra`    | `bool`    |                   | True to pass extra dummy arguments to `aggfinalfn`                                                                                                                                                                                                                                                                                 |
| `aggmfinalextra`   | `bool`    |                   | True to pass extra dummy arguments to `aggmfinalfn`                                                                                                                                                                                                                                                                                |
| `aggsortop`        | `oid`     | `pg_operator`.oid | Associated sort operator (zero if none)                                                                                                                                                                                                                                                                                            |
| `aggtranstype`     | `oid`     | `pg_type`.oid     | Data type of the aggregate function's internal transition (state) data                                                                                                                                                                                                                                                             |
| `aggtransspace`    | `int4`    |                   | Approximate average size (in bytes) of the transition state data, or zero to use a default estimate                                                                                                                                                                                                                                |
| `aggmtranstype`    | `oid`     | `pg_type`.oid     | Data type of the aggregate function's internal transition (state) data for moving-aggregate mode (zero if none)                                                                                                                                                                                                                    |
| `aggmtransspace`   | `int4`    |                   | Approximate average size (in bytes) of the transition state data for moving-aggregate mode, or zero to use a default estimate                                                                                                                                                                                                      |
| `agginitval`       | `text`    |                   | The initial value of the transition state. This is a text field containing the initial value in its external string representation. If this field is null, the transition state value starts out null.                                                                                                                             |
| `aggminitval`      | `text`    |                   | The initial value of the transition state for moving-aggregate mode. This is a text field containing the initial value in its external string representation. If this field is null, the transition state value starts out null.                                                                                                   |

</div>

</div>

  

New aggregate functions are registered with the
[<span class="refentrytitle">CREATE
AGGREGATE</span>](sql-createaggregate.html "CREATE AGGREGATE") command.
See [Section 37.10](xaggr.html "37.10. User-defined Aggregates") for
more information about writing aggregate functions and the meaning of
the transition functions,
etc.

</div>

<div class="navfooter">

-----

|                                |                     |                            |
| :----------------------------- | :-----------------: | -------------------------: |
| [Prev](catalogs-overview.html) | [Up](catalogs.html) | [Next](catalog-pg-am.html) |
| 51.1. Overview                 | [Home](index.html)  |              51.3. `pg_am` |

</div>
