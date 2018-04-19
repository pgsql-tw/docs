<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.20. `pg_enum`

</div>

[Prev](catalog-pg-description.html "51.19. pg_description") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-event-trigger.html "51.21. pg_event_trigger")

-----

<div id="CATALOG-PG-ENUM" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.20. `pg_enum`

</div>

</div>

</div>

<span id="id-1.10.4.22.2" class="indexterm"></span>

The `pg_enum` catalog contains entries showing the values and labels for
each enum type. The internal representation of a given enum value is
actually the OID of its associated row in `pg_enum`.

<div id="id-1.10.4.22.4" class="table">

**Table 51.20. `pg_enum`
Columns**

<div class="table-contents">

| Name            | Type     | References    | Description                                                    |
| --------------- | -------- | ------------- | -------------------------------------------------------------- |
| `oid`           | `oid`    |               | Row identifier (hidden attribute; must be explicitly selected) |
| `enumtypid`     | `oid`    | `pg_type`.oid | The OID of the `pg_type` entry owning this enum value          |
| `enumsortorder` | `float4` |               | The sort position of this enum value within its enum type      |
| `enumlabel`     | `name`   |               | The textual label for this enum value                          |

</div>

</div>

  

The OIDs for `pg_enum` rows follow a special rule: even-numbered OIDs
are guaranteed to be ordered in the same way as the sort ordering of
their enum type. That is, if two even OIDs belong to the same enum type,
the smaller OID must have the smaller `enumsortorder` value.
Odd-numbered OID values need bear no relationship to the sort order.
This rule allows the enum comparison routines to avoid catalog lookups
in many common cases. The routines that create and alter enum types
attempt to assign even OIDs to enum values whenever possible.

When an enum type is created, its members are assigned sort-order
positions 1..*`n`*. But members added later might be given negative or
fractional values of `enumsortorder`. The only requirement on these
values is that they be correctly ordered and unique within each enum
type.

</div>

<div class="navfooter">

-----

|                                     |                     |                                       |
| :---------------------------------- | :-----------------: | ------------------------------------: |
| [Prev](catalog-pg-description.html) | [Up](catalogs.html) | [Next](catalog-pg-event-trigger.html) |
| 51.19. `pg_description`             | [Home](index.html)  |             51.21. `pg_event_trigger` |

</div>
