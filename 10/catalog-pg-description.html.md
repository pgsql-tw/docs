<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.19. `pg_description`

</div>

[Prev](catalog-pg-depend.html "51.18. pg_depend") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-enum.html "51.20. pg_enum")

-----

<div id="CATALOG-PG-DESCRIPTION" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.19. `pg_description`

</div>

</div>

</div>

<span id="id-1.10.4.21.2" class="indexterm"></span>

The catalog `pg_description` stores optional descriptions (comments) for
each database object. Descriptions can be manipulated with the
[<span class="refentrytitle">COMMENT</span>](sql-comment.html "COMMENT")
command and viewed with <span class="application">psql</span>'s `\d`
commands. Descriptions of many built-in system objects are provided in
the initial contents of `pg_description`.

See also
[`pg_shdescription`](catalog-pg-shdescription.html "51.48. pg_shdescription"),
which performs a similar function for descriptions involving objects
that are shared across a database cluster.

<div id="id-1.10.4.21.5" class="table">

**Table 51.19. `pg_description`
Columns**

<div class="table-contents">

| Name          | Type   | References     | Description                                                                                                                                                          |
| ------------- | ------ | -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `objoid`      | `oid`  | any OID column | The OID of the object this description pertains to                                                                                                                   |
| `classoid`    | `oid`  | `pg_class`.oid | The OID of the system catalog this object appears in                                                                                                                 |
| `objsubid`    | `int4` |                | For a comment on a table column, this is the column number (the `objoid` and `classoid` refer to the table itself). For all other object types, this column is zero. |
| `description` | `text` |                | Arbitrary text that serves as the description of this object                                                                                                         |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                                |                     |                              |
| :----------------------------- | :-----------------: | ---------------------------: |
| [Prev](catalog-pg-depend.html) | [Up](catalogs.html) | [Next](catalog-pg-enum.html) |
| 51.18. `pg_depend`             | [Home](index.html)  |             51.20. `pg_enum` |

</div>
