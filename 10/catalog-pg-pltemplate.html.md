<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.37. `pg_pltemplate`

</div>

[Prev](catalog-pg-partitioned-table.html "51.36. pg_partitioned_table") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-policy.html "51.38. pg_policy")

-----

<div id="CATALOG-PG-PLTEMPLATE" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.37. `pg_pltemplate`

</div>

</div>

</div>

<span id="id-1.10.4.39.2" class="indexterm"></span>

The catalog `pg_pltemplate` stores
<span class="quote">“<span class="quote">template</span>”</span>
information for procedural languages. A template for a language allows
the language to be created in a particular database by a simple `CREATE
LANGUAGE` command, with no need to specify implementation details.

Unlike most system catalogs, `pg_pltemplate` is shared across all
databases of a cluster: there is only one copy of `pg_pltemplate` per
cluster, not one per database. This allows the information to be
accessible in each database as it is needed.

<div id="id-1.10.4.39.5" class="table">

**Table 51.37. `pg_pltemplate`
Columns**

<div class="table-contents">

| Name            | Type        | Description                                               |
| --------------- | ----------- | --------------------------------------------------------- |
| `tmplname`      | `name`      | Name of the language this template is for                 |
| `tmpltrusted`   | `boolean`   | True if language is considered trusted                    |
| `tmpldbacreate` | `boolean`   | True if language may be created by a database owner       |
| `tmplhandler`   | `text`      | Name of call handler function                             |
| `tmplinline`    | `text`      | Name of anonymous-block handler function, or null if none |
| `tmplvalidator` | `text`      | Name of validator function, or null if none               |
| `tmpllibrary`   | `text`      | Path of shared library that implements language           |
| `tmplacl`       | `aclitem[]` | Access privileges for template (not actually used)        |

</div>

</div>

  

There are not currently any commands that manipulate procedural language
templates; to change the built-in information, a superuser must modify
the table using ordinary `INSERT`, `DELETE`, or `UPDATE` commands.

<div class="note">

### Note

It is likely that `pg_pltemplate` will be removed in some future release
of <span class="productname">PostgreSQL</span>, in favor of keeping this
knowledge about procedural languages in their respective extension
installation
scripts.

</div>

</div>

<div class="navfooter">

-----

|                                           |                     |                                |
| :---------------------------------------- | :-----------------: | -----------------------------: |
| [Prev](catalog-pg-partitioned-table.html) | [Up](catalogs.html) | [Next](catalog-pg-policy.html) |
| 51.36. `pg_partitioned_table`             | [Home](index.html)  |             51.38. `pg_policy` |

</div>
