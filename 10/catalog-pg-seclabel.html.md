<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.45. `pg_seclabel`

</div>

[Prev](catalog-pg-rewrite.html "51.44. pg_rewrite") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-sequence.html "51.46. pg_sequence")

-----

<div id="CATALOG-PG-SECLABEL" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.45. `pg_seclabel`

</div>

</div>

</div>

<span id="id-1.10.4.47.2" class="indexterm"></span>

The catalog `pg_seclabel` stores security labels on database objects.
Security labels can be manipulated with the
[<span class="refentrytitle">SECURITY
LABEL</span>](sql-security-label.html "SECURITY LABEL") command. For an
easier way to view security labels, see
[Section 51.83](view-pg-seclabels.html "51.83. pg_seclabels").

See also
[`pg_shseclabel`](catalog-pg-shseclabel.html "51.49. pg_shseclabel"),
which performs a similar function for security labels of database
objects that are shared across a database cluster.

<div id="id-1.10.4.47.5" class="table">

**Table 51.45. `pg_seclabel`
Columns**

<div class="table-contents">

| Name       | Type   | References     | Description                                                                                                                                                                 |
| ---------- | ------ | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `objoid`   | `oid`  | any OID column | The OID of the object this security label pertains to                                                                                                                       |
| `classoid` | `oid`  | `pg_class`.oid | The OID of the system catalog this object appears in                                                                                                                        |
| `objsubid` | `int4` |                | For a security label on a table column, this is the column number (the `objoid` and `classoid` refer to the table itself). For all other object types, this column is zero. |
| `provider` | `text` |                | The label provider associated with this label.                                                                                                                              |
| `label`    | `text` |                | The security label applied to this object.                                                                                                                                  |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                 |                     |                                  |
| :------------------------------ | :-----------------: | -------------------------------: |
| [Prev](catalog-pg-rewrite.html) | [Up](catalogs.html) | [Next](catalog-pg-sequence.html) |
| 51.44. `pg_rewrite`             | [Home](index.html)  |             51.46. `pg_sequence` |

</div>
