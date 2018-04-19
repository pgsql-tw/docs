<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.30. `pg_largeobject`

</div>

[Prev](catalog-pg-language.html "51.29. pg_language") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System
Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-largeobject-metadata.html "51.31. pg_largeobject_metadata")

-----

<div id="CATALOG-PG-LARGEOBJECT" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.30. `pg_largeobject`

</div>

</div>

</div>

<span id="id-1.10.4.32.2" class="indexterm"></span>

The catalog `pg_largeobject` holds the data making up
<span class="quote">“<span class="quote">large objects</span>”</span>. A
large object is identified by an OID assigned when it is created. Each
large object is broken into segments or
<span class="quote">“<span class="quote">pages</span>”</span> small
enough to be conveniently stored as rows in `pg_largeobject`. The amount
of data per page is defined to be `LOBLKSIZE` (which is currently
`BLCKSZ/4`, or typically 2 kB).

Prior to <span class="productname">PostgreSQL</span> 9.0, there was no
permission structure associated with large objects. As a result,
`pg_largeobject` was publicly readable and could be used to obtain the
OIDs (and contents) of all large objects in the system. This is no
longer the case; use
[`pg_largeobject_metadata`](catalog-pg-largeobject-metadata.html "51.31. pg_largeobject_metadata")
to obtain a list of large object OIDs.

<div id="id-1.10.4.32.5" class="table">

**Table 51.30. `pg_largeobject`
Columns**

<div class="table-contents">

| Name     | Type    | References                    | Description                                                                                               |
| -------- | ------- | ----------------------------- | --------------------------------------------------------------------------------------------------------- |
| `loid`   | `oid`   | `pg_largeobject_metadata`.oid | Identifier of the large object that includes this page                                                    |
| `pageno` | `int4`  |                               | Page number of this page within its large object (counting from zero)                                     |
| `data`   | `bytea` |                               | Actual data stored in the large object. This will never be more than `LOBLKSIZE` bytes and might be less. |

</div>

</div>

  

Each row of `pg_largeobject` holds data for one page of a large object,
beginning at byte offset (`pageno * LOBLKSIZE`) within the object. The
implementation allows sparse storage: pages might be missing, and might
be shorter than `LOBLKSIZE` bytes even if they are not the last page of
the object. Missing regions within a large object read as
zeroes.

</div>

<div class="navfooter">

-----

|                                  |                     |                                              |
| :------------------------------- | :-----------------: | -------------------------------------------: |
| [Prev](catalog-pg-language.html) | [Up](catalogs.html) | [Next](catalog-pg-largeobject-metadata.html) |
| 51.29. `pg_language`             | [Home](index.html)  |             51.31. `pg_largeobject_metadata` |

</div>
