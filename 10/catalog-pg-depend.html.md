<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.18. `pg_depend`

</div>

[Prev](catalog-pg-default-acl.html "51.17. pg_default_acl") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-description.html "51.19. pg_description")

-----

<div id="CATALOG-PG-DEPEND" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.18. `pg_depend`

</div>

</div>

</div>

<span id="id-1.10.4.20.2" class="indexterm"></span>

The catalog `pg_depend` records the dependency relationships between
database objects. This information allows `DROP` commands to find which
other objects must be dropped by `DROP CASCADE` or prevent dropping in
the `DROP RESTRICT` case.

See also [`pg_shdepend`](catalog-pg-shdepend.html "51.47. pg_shdepend"),
which performs a similar function for dependencies involving objects
that are shared across a database cluster.

<div id="id-1.10.4.20.5" class="table">

**Table 51.18. `pg_depend`
Columns**

<div class="table-contents">

| Name          | Type   | References     | Description                                                                                                                                                 |
| ------------- | ------ | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `classid`     | `oid`  | `pg_class`.oid | The OID of the system catalog the dependent object is in                                                                                                    |
| `objid`       | `oid`  | any OID column | The OID of the specific dependent object                                                                                                                    |
| `objsubid`    | `int4` |                | For a table column, this is the column number (the `objid` and `classid` refer to the table itself). For all other object types, this column is zero.       |
| `refclassid`  | `oid`  | `pg_class`.oid | The OID of the system catalog the referenced object is in                                                                                                   |
| `refobjid`    | `oid`  | any OID column | The OID of the specific referenced object                                                                                                                   |
| `refobjsubid` | `int4` |                | For a table column, this is the column number (the `refobjid` and `refclassid` refer to the table itself). For all other object types, this column is zero. |
| `deptype`     | `char` |                | A code defining the specific semantics of this dependency relationship; see text                                                                            |

</div>

</div>

  

In all cases, a `pg_depend` entry indicates that the referenced object
cannot be dropped without also dropping the dependent object. However,
there are several subflavors identified by `deptype`:

<div class="variablelist">

  - <span class="term">`DEPENDENCY_NORMAL` (`n`)</span>  
    A normal relationship between separately-created objects. The
    dependent object can be dropped without affecting the referenced
    object. The referenced object can only be dropped by specifying
    `CASCADE`, in which case the dependent object is dropped, too.
    Example: a table column has a normal dependency on its data type.

  - <span class="term">`DEPENDENCY_AUTO` (`a`)</span>  
    The dependent object can be dropped separately from the referenced
    object, and should be automatically dropped (regardless of
    `RESTRICT` or `CASCADE` mode) if the referenced object is dropped.
    Example: a named constraint on a table is made autodependent on the
    table, so that it will go away if the table is dropped.

  - <span class="term">`DEPENDENCY_INTERNAL` (`i`)</span>  
    The dependent object was created as part of creation of the
    referenced object, and is really just a part of its internal
    implementation. A `DROP` of the dependent object will be disallowed
    outright (we'll tell the user to issue a `DROP` against the
    referenced object, instead). A `DROP` of the referenced object will
    be propagated through to drop the dependent object whether `CASCADE`
    is specified or not. Example: a trigger that's created to enforce a
    foreign-key constraint is made internally dependent on the
    constraint's `pg_constraint` entry.

  - <span class="term">`DEPENDENCY_EXTENSION` (`e`)</span>  
    The dependent object is a member of the *extension* that is the
    referenced object (see
    [`pg_extension`](catalog-pg-extension.html "51.22. pg_extension")).
    The dependent object can be dropped only via `DROP EXTENSION` on the
    referenced object. Functionally this dependency type acts the same
    as an internal dependency, but it's kept separate for clarity and to
    simplify <span class="application">pg\_dump</span>.

  - <span class="term">`DEPENDENCY_AUTO_EXTENSION` (`x`)</span>  
    The dependent object is not a member of the extension that is the
    referenced object (and so should not be ignored by pg\_dump), but
    cannot function without it and should be dropped when the extension
    itself is. The dependent object may be dropped on its own as well.

  - <span class="term">`DEPENDENCY_PIN` (`p`)</span>  
    There is no dependent object; this type of entry is a signal that
    the system itself depends on the referenced object, and so that
    object must never be deleted. Entries of this type are created only
    by `initdb`. The columns for the dependent object contain zeroes.

</div>

Other dependency flavors might be needed in
future.

</div>

<div class="navfooter">

-----

|                                     |                     |                                     |
| :---------------------------------- | :-----------------: | ----------------------------------: |
| [Prev](catalog-pg-default-acl.html) | [Up](catalogs.html) | [Next](catalog-pg-description.html) |
| 51.17. `pg_default_acl`             | [Home](index.html)  |             51.19. `pg_description` |

</div>
