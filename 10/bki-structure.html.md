<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

67.3. Structure of the Bootstrap BKI File

</div>

[Prev](bki-commands.html "67.2. BKI Commands") 

[Up](bki.html "Chapter 67. BKI Backend Interface")

Chapter 67. BKI Backend Interface

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](bki-example.html "67.4. Example")

-----

<div id="BKI-STRUCTURE" class="sect1">

<div class="titlepage">

<div>

<div>

## 67.3. Structure of the Bootstrap BKI File

</div>

</div>

</div>

The `open` command cannot be used until the tables it uses exist and
have entries for the table that is to be opened. (These minimum tables
are `pg_class`, `pg_attribute`, `pg_proc`, and `pg_type`.) To allow
those tables themselves to be filled, `create` with the `bootstrap`
option implicitly opens the created table for data insertion.

Also, the `declare index` and `declare toast` commands cannot be used
until the system catalogs they need have been created and filled in.

Thus, the structure of the `postgres.bki` file has to be:

<div class="orderedlist">

1.  `create bootstrap` one of the critical tables

2.  `insert` data describing at least the critical tables

3.  `close`

4.  Repeat for the other critical tables.

5.  `create` (without `bootstrap`) a noncritical table

6.  `open`

7.  `insert` desired data

8.  `close`

9.  Repeat for the other noncritical tables.

10. Define indexes and toast tables.

11. `build indices`

</div>

There are doubtless other, undocumented ordering
dependencies.

</div>

<div class="navfooter">

-----

|                           |                    |                          |
| :------------------------ | :----------------: | -----------------------: |
| [Prev](bki-commands.html) |   [Up](bki.html)   | [Next](bki-example.html) |
| 67.2. BKI Commands        | [Home](index.html) |            67.4. Example |

</div>
