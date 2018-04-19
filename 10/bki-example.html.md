<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

67.4. Example

</div>

[Prev](bki-structure.html "67.3. Structure of the Bootstrap BKI File") 

[Up](bki.html "Chapter 67. BKI Backend Interface")

Chapter 67. BKI Backend
Interface

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](planner-stats-details.html "Chapter 68. How the Planner Uses Statistics")

-----

<div id="BKI-EXAMPLE" class="sect1">

<div class="titlepage">

<div>

<div>

## 67.4. Example

</div>

</div>

</div>

The following sequence of commands will create the table `test_table`
with OID 420, having two columns `cola` and `colb` of type `int4` and
`text`, respectively, and insert two rows into the table:

``` programlisting
create test_table 420 (cola = int4, colb = text)
open test_table
insert OID=421 ( 1 "value1" )
insert OID=422 ( 2 _null_ )
close test_table
```

</div>

<div class="navfooter">

-----

|                                           |                    |                                             |
| :---------------------------------------- | :----------------: | ------------------------------------------: |
| [Prev](bki-structure.html)                |   [Up](bki.html)   |          [Next](planner-stats-details.html) |
| 67.3. Structure of the Bootstrap BKI File | [Home](index.html) | Chapter 68. How the Planner Uses Statistics |

</div>
