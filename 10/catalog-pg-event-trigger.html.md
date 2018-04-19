<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

51.21. `pg_event_trigger`

</div>

[Prev](catalog-pg-enum.html "51.20. pg_enum") 

[Up](catalogs.html "Chapter 51. System Catalogs")

Chapter 51. System Catalogs

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](catalog-pg-extension.html "51.22. pg_extension")

-----

<div id="CATALOG-PG-EVENT-TRIGGER" class="sect1">

<div class="titlepage">

<div>

<div>

## 51.21. `pg_event_trigger`

</div>

</div>

</div>

<span id="id-1.10.4.23.2" class="indexterm"></span>

The catalog `pg_event_trigger` stores event triggers. See
[Chapter 39](event-triggers.html "Chapter 39. Event Triggers") for more
information.

<div id="id-1.10.4.23.4" class="table">

**Table 51.21. `pg_event_trigger`
Columns**

<div class="table-contents">

| Name         | Type     | References      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------ | -------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `evtname`    | `name`   |                 | Trigger name (must be unique)                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `evtevent`   | `name`   |                 | Identifies the event for which this trigger fires                                                                                                                                                                                                                                                                                                                                                                                                         |
| `evtowner`   | `oid`    | `pg_authid`.oid | Owner of the event trigger                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `evtfoid`    | `oid`    | `pg_proc`.oid   | The function to be called                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `evtenabled` | `char`   |                 | Controls in which [session\_replication\_role](runtime-config-client.html#GUC-SESSION-REPLICATION-ROLE) modes the event trigger fires. `O` = trigger fires in <span class="quote">“<span class="quote">origin</span>”</span> and <span class="quote">“<span class="quote">local</span>”</span> modes, `D` = trigger is disabled, `R` = trigger fires in <span class="quote">“<span class="quote">replica</span>”</span> mode, `A` = trigger fires always. |
| `evttags`    | `text[]` |                 | Command tags for which this trigger will fire. If NULL, the firing of this trigger is not restricted on the basis of the command tag.                                                                                                                                                                                                                                                                                                                     |

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                              |                     |                                   |
| :--------------------------- | :-----------------: | --------------------------------: |
| [Prev](catalog-pg-enum.html) | [Up](catalogs.html) | [Next](catalog-pg-extension.html) |
| 51.20. `pg_enum`             | [Home](index.html)  |             51.22. `pg_extension` |

</div>
