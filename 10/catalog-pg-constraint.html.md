<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.13. `pg_constraint`

</div>

[Prev](catalog-pg-collation.html "51.12. pg_collation") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-conversion.html "51.14. pg_conversion")

-----

<div id="CATALOG-PG-CONSTRAINT" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.13. `pg_constraint`

</div>

</div>

</div>

<span id="id-1.10.4.15.2" class="indexterm"></span>

The catalog `pg_constraint` stores check, primary key, unique, foreign
key, and exclusion constraints on tables. (Column constraints are not
treated specially. Every column constraint is equivalent to some table
constraint.) Not-null constraints are represented in the `pg_attribute`
catalog, not here.

User-defined constraint triggers (created with `CREATE CONSTRAINT
TRIGGER`) also give rise to an entry in this table.

Check constraints on domains are stored here, too.

<div id="id-1.10.4.15.6" class="table">

**Table 51.13. `pg_constraint`
Columns**

<div class="table-contents">

| Name            | Type           | References            | Description                                                                                                                                                       |
| --------------- | -------------- | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `oid`           | `oid`          |                       | Row identifier (hidden attribute; must be explicitly selected)                                                                                                    |
| `conname`       | `name`         |                       | Constraint name (not necessarily unique\!)                                                                                                                        |
| `connamespace`  | `oid`          | `pg_namespace`.oid    | The OID of the namespace that contains this constraint                                                                                                            |
| `contype`       | `char`         |                       | `c` = check constraint, `f` = foreign key constraint, `p` = primary key constraint, `u` = unique constraint, `t` = constraint trigger, `x` = exclusion constraint |
| `condeferrable` | `bool`         |                       | Is the constraint deferrable?                                                                                                                                     |
| `condeferred`   | `bool`         |                       | Is the constraint deferred by default?                                                                                                                            |
| `convalidated`  | `bool`         |                       | Has the constraint been validated? Currently, can only be false for foreign keys and CHECK constraints                                                            |
| `conrelid`      | `oid`          | `pg_class`.oid        | The table this constraint is on; 0 if not a table constraint                                                                                                      |
| `contypid`      | `oid`          | `pg_type`.oid         | The domain this constraint is on; 0 if not a domain constraint                                                                                                    |
| `conindid`      | `oid`          | `pg_class`.oid        | The index supporting this constraint, if it's a unique, primary key, foreign key, or exclusion constraint; else 0                                                 |
| `confrelid`     | `oid`          | `pg_class`.oid        | If a foreign key, the referenced table; else 0                                                                                                                    |
| `confupdtype`   | `char`         |                       | Foreign key update action code: `a` = no action, `r` = restrict, `c` = cascade, `n` = set null, `d` = set default                                                 |
| `confdeltype`   | `char`         |                       | Foreign key deletion action code: `a` = no action, `r` = restrict, `c` = cascade, `n` = set null, `d` = set default                                               |
| `confmatchtype` | `char`         |                       | Foreign key match type: `f` = full, `p` = partial, `s` = simple                                                                                                   |
| `conislocal`    | `bool`         |                       | This constraint is defined locally for the relation. Note that a constraint can be locally defined and inherited simultaneously.                                  |
| `coninhcount`   | `int4`         |                       | The number of direct inheritance ancestors this constraint has. A constraint with a nonzero number of ancestors cannot be dropped nor renamed.                    |
| `connoinherit`  | `bool`         |                       | This constraint is defined locally for the relation. It is a non-inheritable constraint.                                                                          |
| `conkey`        | `int2[]`       | `pg_attribute`.attnum | If a table constraint (including foreign keys, but not constraint triggers), list of the constrained columns                                                      |
| `confkey`       | `int2[]`       | `pg_attribute`.attnum | If a foreign key, list of the referenced columns                                                                                                                  |
| `conpfeqop`     | `oid[]`        | `pg_operator`.oid     | If a foreign key, list of the equality operators for PK = FK comparisons                                                                                          |
| `conppeqop`     | `oid[]`        | `pg_operator`.oid     | If a foreign key, list of the equality operators for PK = PK comparisons                                                                                          |
| `conffeqop`     | `oid[]`        | `pg_operator`.oid     | If a foreign key, list of the equality operators for FK = FK comparisons                                                                                          |
| `conexclop`     | `oid[]`        | `pg_operator`.oid     | If an exclusion constraint, list of the per-column exclusion operators                                                                                            |
| `conbin`        | `pg_node_tree` |                       | If a check constraint, an internal representation of the expression                                                                                               |
| `consrc`        | `text`         |                       | If a check constraint, a human-readable representation of the expression                                                                                          |

</div>

</div>

  

In the case of an exclusion constraint, `conkey` is only useful for
constraint elements that are simple column references. For other cases,
a zero appears in `conkey` and the associated index must be consulted to
discover the expression that is constrained. (`conkey` thus has the same
contents as `pg_index`.`indkey` for the index.)

<div class="note">

### Note

`consrc` is not updated when referenced objects change; for example, it
won't track renaming of columns. Rather than relying on this field, it's
best to use `pg_get_constraintdef()` to extract the definition of a
check constraint.

</div>

<div class="note">

### Note

`pg_class.relchecks` needs to agree with the number of check-constraint
entries found in this table for each
relation.

</div>

</div>

<div class="navfooter">

-----

|                                   |                     |                                    |
| :-------------------------------- | :-----------------: | ---------------------------------: |
| [Prev](catalog-pg-collation.html) | [Up](catalogs.html) | [Next](catalog-pg-conversion.html) |
| 51.12. `pg_collation`             | [Home](index.html)  |             51.14. `pg_conversion` |

</div>
