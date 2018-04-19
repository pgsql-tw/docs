<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

F.7. btree\_gist

</div>

[Prev](btree-gin.html "F.6. btree_gin") 

[Up](contrib.html "Appendix F. Additional Supplied Modules")

Appendix F. Additional Supplied Modules

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](chkpass.html "F.8. chkpass")

-----

<div id="BTREE-GIST" class="sect1">

<div class="titlepage">

<div>

<div>

## F.7. btree\_gist

</div>

</div>

</div>

<div class="toc">

<span class="sect2">[F.7.1. Example
Usage](btree-gist.html#id-1.11.7.16.7)</span>

<span class="sect2">[F.7.2.
Authors](btree-gist.html#id-1.11.7.16.8)</span>

</div>

<span id="id-1.11.7.16.2" class="indexterm"></span>

`btree_gist` provides GiST index operator classes that implement B-tree
equivalent behavior for the data types `int2`, `int4`, `int8`, `float4`,
`float8`, `numeric`, `timestamp with time zone`, `timestamp without time
zone`, `time with time zone`, `time without time zone`, `date`,
`interval`, `oid`, `money`, `char`, `varchar`, `text`, `bytea`, `bit`,
`varbit`, `macaddr`, `macaddr8`, `inet`, `cidr`, `uuid`, and all `enum`
types.

In general, these operator classes will not outperform the equivalent
standard B-tree index methods, and they lack one major feature of the
standard B-tree code: the ability to enforce uniqueness. However, they
provide some other features that are not available with a B-tree index,
as described below. Also, these operator classes are useful when a
multicolumn GiST index is needed, wherein some of the columns are of
data types that are only indexable with GiST but other columns are just
simple data types. Lastly, these operator classes are useful for GiST
testing and as a base for developing other GiST operator classes.

In addition to the typical B-tree search operators, `btree_gist` also
provides index support for `<>`
(<span class="quote">“<span class="quote">not equals</span>”</span>).
This may be useful in combination with an [exclusion
constraint](sql-createtable.html#SQL-CREATETABLE-EXCLUDE), as described
below.

Also, for data types for which there is a natural distance metric,
`btree_gist` defines a distance operator `<->`, and provides GiST index
support for nearest-neighbor searches using this operator. Distance
operators are provided for `int2`, `int4`, `int8`, `float4`, `float8`,
`timestamp with time zone`, `timestamp without time zone`, `time without
time zone`, `date`, `interval`, `oid`, and `money`.

<div id="id-1.11.7.16.7" class="sect2">

<div class="titlepage">

<div>

<div>

### F.7.1. Example Usage

</div>

</div>

</div>

Simple example using `btree_gist` instead of `btree`:

``` programlisting
CREATE TABLE test (a int4);
-- create index
CREATE INDEX testidx ON test USING GIST (a);
-- query
SELECT * FROM test WHERE a < 10;
-- nearest-neighbor search: find the ten entries closest to "42"
SELECT *, a <-> 42 AS dist FROM test ORDER BY a <-> 42 LIMIT 10;
```

Use an [exclusion
constraint](sql-createtable.html#SQL-CREATETABLE-EXCLUDE) to enforce the
rule that a cage at a zoo can contain only one kind of animal:

``` programlisting
=> CREATE TABLE zoo (
  cage   INTEGER,
  animal TEXT,
  EXCLUDE USING GIST (cage WITH =, animal WITH <>)
);

=> INSERT INTO zoo VALUES(123, 'zebra');
INSERT 0 1
=> INSERT INTO zoo VALUES(123, 'zebra');
INSERT 0 1
=> INSERT INTO zoo VALUES(123, 'lion');
ERROR:  conflicting key value violates exclusion constraint "zoo_cage_animal_excl"
DETAIL:  Key (cage, animal)=(123, lion) conflicts with existing key (cage, animal)=(123, zebra).
=> INSERT INTO zoo VALUES(124, 'lion');
INSERT 0 1
```

</div>

<div id="id-1.11.7.16.8" class="sect2">

<div class="titlepage">

<div>

<div>

### F.7.2. Authors

</div>

</div>

</div>

Teodor Sigaev (`<teodor@stack.net>`), Oleg Bartunov
(`<oleg@sai.msu.su>`), Janko Richter (`<jankorichter@yahoo.de>`), and
Paul Jungwirth (`<pj@illuminatedcomputing.com>`). See
<http://www.sai.msu.su/~megera/postgres/gist/> for additional
information.

</div>

</div>

<div class="navfooter">

-----

|                        |                    |                      |
| :--------------------- | :----------------: | -------------------: |
| [Prev](btree-gin.html) | [Up](contrib.html) | [Next](chkpass.html) |
| F.6. btree\_gin        | [Home](index.html) |         F.8. chkpass |

</div>
