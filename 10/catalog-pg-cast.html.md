<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.10. `pg_cast`

</div>

[Prev](catalog-pg-auth-members.html "51.9. pg_auth_members") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-class.html "51.11. pg_class")

-----

<div id="CATALOG-PG-CAST" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.10. `pg_cast`

</div>

</div>

</div>

<span id="id-1.10.4.12.2" class="indexterm"></span>

The catalog `pg_cast` stores data type conversion paths, both built-in
and user-defined.

It should be noted that `pg_cast` does not represent every type
conversion that the system knows how to perform; only those that cannot
be deduced from some generic rule. For example, casting between a domain
and its base type is not explicitly represented in `pg_cast`. Another
important exception is that
<span class="quote">“<span class="quote">automatic I/O conversion
casts</span>”</span>, those performed using a data type's own I/O
functions to convert to or from `text` or other string types, are not
explicitly represented in `pg_cast`.

<div id="id-1.10.4.12.5" class="table">

**Table 51.10. `pg_cast`
Columns**

<div class="table-contents">

| Name          | Type   | References    | Description                                                                                                                                                                                                                                                          |
| ------------- | ------ | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `oid`         | `oid`  |               | Row identifier (hidden attribute; must be explicitly selected)                                                                                                                                                                                                       |
| `castsource`  | `oid`  | `pg_type`.oid | OID of the source data type                                                                                                                                                                                                                                          |
| `casttarget`  | `oid`  | `pg_type`.oid | OID of the target data type                                                                                                                                                                                                                                          |
| `castfunc`    | `oid`  | `pg_proc`.oid | The OID of the function to use to perform this cast. Zero is stored if the cast method doesn't require a function.                                                                                                                                                   |
| `castcontext` | `char` |               | Indicates what contexts the cast can be invoked in. `e` means only as an explicit cast (using `CAST` or `::` syntax). `a` means implicitly in assignment to a target column, as well as explicitly. `i` means implicitly in expressions, as well as the other cases. |
| `castmethod`  | `char` |               | Indicates how the cast is performed. `f` means that the function specified in the `castfunc` field is used. `i` means that the input/output functions are used. `b` means that the types are binary-coercible, thus no conversion is required.                       |

</div>

</div>

  

The cast functions listed in `pg_cast` must always take the cast source
type as their first argument type, and return the cast destination type
as their result type. A cast function can have up to three arguments.
The second argument, if present, must be type `integer`; it receives the
type modifier associated with the destination type, or -1 if there is
none. The third argument, if present, must be type `boolean`; it
receives `true` if the cast is an explicit cast, `false` otherwise.

It is legitimate to create a `pg_cast` entry in which the source and
target types are the same, if the associated function takes more than
one argument. Such entries represent
<span class="quote">“<span class="quote">length coercion
functions</span>”</span> that coerce values of the type to be legal for
a particular type modifier value.

When a `pg_cast` entry has different source and target types and a
function that takes more than one argument, it represents converting
from one type to another and applying a length coercion in a single
step. When no such entry is available, coercion to a type that uses a
type modifier involves two steps, one to convert between data types and
a second to apply the
modifier.

</div>

<div class="navfooter">

-----

|                                      |                     |                               |
| :----------------------------------- | :-----------------: | ----------------------------: |
| [Prev](catalog-pg-auth-members.html) | [Up](catalogs.html) | [Next](catalog-pg-class.html) |
| 51.9. `pg_auth_members`              | [Home](index.html)  |             51.11. `pg_class` |

</div>
