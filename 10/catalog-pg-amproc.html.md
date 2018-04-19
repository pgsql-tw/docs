<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.5. `pg_amproc`

</div>

[Prev](catalog-pg-amop.html "51.4. pg_amop") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-attrdef.html "51.6. pg_attrdef")

-----

<div id="CATALOG-PG-AMPROC" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.5. `pg_amproc`

</div>

</div>

</div>

<span id="id-1.10.4.7.2" class="indexterm"></span>

The catalog `pg_amproc` stores information about support procedures
associated with access method operator families. There is one row for
each support procedure belonging to an operator family.

<div id="id-1.10.4.7.4" class="table">

**Table 51.5. `pg_amproc`
Columns**

<div class="table-contents">

| Name              | Type      | References        | Description                                                    |
| ----------------- | --------- | ----------------- | -------------------------------------------------------------- |
| `oid`             | `oid`     |                   | Row identifier (hidden attribute; must be explicitly selected) |
| `amprocfamily`    | `oid`     | `pg_opfamily`.oid | The operator family this entry is for                          |
| `amproclefttype`  | `oid`     | `pg_type`.oid     | Left-hand input data type of associated operator               |
| `amprocrighttype` | `oid`     | `pg_type`.oid     | Right-hand input data type of associated operator              |
| `amprocnum`       | `int2`    |                   | Support procedure number                                       |
| `amproc`          | `regproc` | `pg_proc`.oid     | OID of the procedure                                           |

</div>

</div>

  

The usual interpretation of the `amproclefttype` and `amprocrighttype`
fields is that they identify the left and right input types of the
operator(s) that a particular support procedure supports. For some
access methods these match the input data type(s) of the support
procedure itself, for others not. There is a notion of
<span class="quote">“<span class="quote">default</span>”</span> support
procedures for an index, which are those with `amproclefttype` and
`amprocrighttype` both equal to the index operator class's
`opcintype`.

</div>

<div class="navfooter">

-----

|                              |                     |                                 |
| :--------------------------- | :-----------------: | ------------------------------: |
| [Prev](catalog-pg-amop.html) | [Up](catalogs.html) | [Next](catalog-pg-attrdef.html) |
| 51.4. `pg_amop`              | [Home](index.html)  |              51.6. `pg_attrdef` |

</div>
