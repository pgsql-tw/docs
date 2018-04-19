<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

F.5. bloom

</div>

[Prev](auto-explain.html "F.4. auto_explain") 

[Up](contrib.html "Appendix F. Additional Supplied Modules")

Appendix F. Additional Supplied Modules

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](btree-gin.html "F.6. btree_gin")

-----

<div id="BLOOM" class="sect1">

<div class="titlepage">

<div>

<div>

## F.5. bloom

</div>

</div>

</div>

<div class="toc">

<span class="sect2">[F.5.1.
Parameters](bloom.html#id-1.11.7.14.7)</span>

<span class="sect2">[F.5.2. Examples](bloom.html#id-1.11.7.14.8)</span>

<span class="sect2">[F.5.3. Operator Class
Interface](bloom.html#id-1.11.7.14.9)</span>

<span class="sect2">[F.5.4.
Limitations](bloom.html#id-1.11.7.14.10)</span>

<span class="sect2">[F.5.5. Authors](bloom.html#id-1.11.7.14.11)</span>

</div>

<span id="id-1.11.7.14.2" class="indexterm"></span>

`bloom` provides an index access method based on [Bloom
filters](http://en.wikipedia.org/wiki/Bloom_filter).

A Bloom filter is a space-efficient data structure that is used to test
whether an element is a member of a set. In the case of an index access
method, it allows fast exclusion of non-matching tuples via signatures
whose size is determined at index creation.

A signature is a lossy representation of the indexed attribute(s), and
as such is prone to reporting false positives; that is, it may be
reported that an element is in the set, when it is not. So index search
results must always be rechecked using the actual attribute values from
the heap entry. Larger signatures reduce the odds of a false positive
and thus reduce the number of useless heap visits, but of course also
make the index larger and hence slower to scan.

This type of index is most useful when a table has many attributes and
queries test arbitrary combinations of them. A traditional btree index
is faster than a bloom index, but it can require many btree indexes to
support all possible queries where one needs only a single bloom index.
Note however that bloom indexes only support equality queries, whereas
btree indexes can also perform inequality and range searches.

<div id="id-1.11.7.14.7" class="sect2">

<div class="titlepage">

<div>

<div>

### F.5.1. Parameters

</div>

</div>

</div>

A `bloom` index accepts the following parameters in its `WITH` clause:

<div class="variablelist">

  - <span class="term">`length`</span>  
    Length of each signature (index entry) in bits. The default is `80`
    bits and maximum is `4096`.

</div>

<div class="variablelist">

  - <span class="term">`col1 — col32`</span>  
    Number of bits generated for each index column. Each parameter's
    name refers to the number of the index column that it controls. The
    default is `2` bits and maximum is `4095`. Parameters for index
    columns not actually used are ignored.

</div>

</div>

<div id="id-1.11.7.14.8" class="sect2">

<div class="titlepage">

<div>

<div>

### F.5.2. Examples

</div>

</div>

</div>

This is an example of creating a bloom index:

``` programlisting
CREATE INDEX bloomidx ON tbloom USING bloom (i1,i2,i3)
       WITH (length=80, col1=2, col2=2, col3=4);
```

The index is created with a signature length of 80 bits, with attributes
i1 and i2 mapped to 2 bits, and attribute i3 mapped to 4 bits. We could
have omitted the `length`, `col1`, and `col2` specifications since those
have the default values.

Here is a more complete example of bloom index definition and usage, as
well as a comparison with equivalent btree indexes. The bloom index is
considerably smaller than the btree index, and can perform better.

``` programlisting
=# CREATE TABLE tbloom AS
   SELECT
     (random() * 1000000)::int as i1,
     (random() * 1000000)::int as i2,
     (random() * 1000000)::int as i3,
     (random() * 1000000)::int as i4,
     (random() * 1000000)::int as i5,
     (random() * 1000000)::int as i6
   FROM
  generate_series(1,10000000);
SELECT 10000000
=# CREATE INDEX bloomidx ON tbloom USING bloom (i1, i2, i3, i4, i5, i6);
CREATE INDEX
=# SELECT pg_size_pretty(pg_relation_size('bloomidx'));
 pg_size_pretty
----------------
 153 MB
(1 row)
=# CREATE index btreeidx ON tbloom (i1, i2, i3, i4, i5, i6);
CREATE INDEX
=# SELECT pg_size_pretty(pg_relation_size('btreeidx'));
 pg_size_pretty
----------------
 387 MB
(1 row)
```

A sequential scan over this large table takes a long
time:

``` programlisting
=# EXPLAIN ANALYZE SELECT * FROM tbloom WHERE i2 = 898732 AND i5 = 123451;
                                                 QUERY PLAN
------------------------------------------------------------------------------------------------------------
 Seq Scan on tbloom  (cost=0.00..213694.08 rows=1 width=24) (actual time=1445.438..1445.438 rows=0 loops=1)
   Filter: ((i2 = 898732) AND (i5 = 123451))
   Rows Removed by Filter: 10000000
 Planning time: 0.177 ms
 Execution time: 1445.473 ms
(5 rows)
```

So the planner will usually select an index scan if possible. With a
btree index, we get results like
this:

``` programlisting
=# EXPLAIN ANALYZE SELECT * FROM tbloom WHERE i2 = 898732 AND i5 = 123451;
                                                           QUERY PLAN
--------------------------------------------------------------------------------------------------------------------------------
 Index Only Scan using btreeidx on tbloom  (cost=0.56..298311.96 rows=1 width=24) (actual time=445.709..445.709 rows=0 loops=1)
   Index Cond: ((i2 = 898732) AND (i5 = 123451))
   Heap Fetches: 0
 Planning time: 0.193 ms
 Execution time: 445.770 ms
(5 rows)
```

Bloom is better than btree in handling this type of
search:

``` programlisting
=# EXPLAIN ANALYZE SELECT * FROM tbloom WHERE i2 = 898732 AND i5 = 123451;
                                                        QUERY PLAN
---------------------------------------------------------------------------------------------------------------------------
 Bitmap Heap Scan on tbloom  (cost=178435.39..178439.41 rows=1 width=24) (actual time=76.698..76.698 rows=0 loops=1)
   Recheck Cond: ((i2 = 898732) AND (i5 = 123451))
   Rows Removed by Index Recheck: 2439
   Heap Blocks: exact=2408
   ->  Bitmap Index Scan on bloomidx  (cost=0.00..178435.39 rows=1 width=0) (actual time=72.455..72.455 rows=2439 loops=1)
         Index Cond: ((i2 = 898732) AND (i5 = 123451))
 Planning time: 0.475 ms
 Execution time: 76.778 ms
(8 rows)
```

Note the relatively large number of false positives: 2439 rows were
selected to be visited in the heap, but none actually matched the query.
We could reduce that by specifying a larger signature length. In this
example, creating the index with `length=200` reduced the number of
false positives to 55; but it doubled the index size (to 306 MB) and
ended up being slower for this query (125 ms overall).

Now, the main problem with the btree search is that btree is inefficient
when the search conditions do not constrain the leading index column(s).
A better strategy for btree is to create a separate index on each
column. Then the planner will choose something like
this:

``` programlisting
=# EXPLAIN ANALYZE SELECT * FROM tbloom WHERE i2 = 898732 AND i5 = 123451;
                                                          QUERY PLAN
------------------------------------------------------------------------------------------------------------------------------
 Bitmap Heap Scan on tbloom  (cost=9.29..13.30 rows=1 width=24) (actual time=0.148..0.148 rows=0 loops=1)
   Recheck Cond: ((i5 = 123451) AND (i2 = 898732))
   ->  BitmapAnd  (cost=9.29..9.29 rows=1 width=0) (actual time=0.145..0.145 rows=0 loops=1)
         ->  Bitmap Index Scan on tbloom_i5_idx  (cost=0.00..4.52 rows=11 width=0) (actual time=0.089..0.089 rows=10 loops=1)
               Index Cond: (i5 = 123451)
         ->  Bitmap Index Scan on tbloom_i2_idx  (cost=0.00..4.52 rows=11 width=0) (actual time=0.048..0.048 rows=8 loops=1)
               Index Cond: (i2 = 898732)
 Planning time: 2.049 ms
 Execution time: 0.280 ms
(9 rows)
```

Although this query runs much faster than with either of the single
indexes, we pay a large penalty in index size. Each of the single-column
btree indexes occupies 214 MB, so the total space needed is over 1.2GB,
more than 8 times the space used by the bloom index.

</div>

<div id="id-1.11.7.14.9" class="sect2">

<div class="titlepage">

<div>

<div>

### F.5.3. Operator Class Interface

</div>

</div>

</div>

An operator class for bloom indexes requires only a hash function for
the indexed data type and an equality operator for searching. This
example shows the operator class definition for the `text` data type:

``` programlisting
CREATE OPERATOR CLASS text_ops
DEFAULT FOR TYPE text USING bloom AS
    OPERATOR    1   =(text, text),
    FUNCTION    1   hashtext(text);
```

</div>

<div id="id-1.11.7.14.10" class="sect2">

<div class="titlepage">

<div>

<div>

### F.5.4. Limitations

</div>

</div>

</div>

<div class="itemizedlist">

  - Only operator classes for `int4` and `text` are included with the
    module.

  - Only the `=` operator is supported for search. But it is possible to
    add support for arrays with union and intersection operations in the
    future.

</div>

</div>

<div id="id-1.11.7.14.11" class="sect2">

<div class="titlepage">

<div>

<div>

### F.5.5. Authors

</div>

</div>

</div>

Teodor Sigaev `<teodor@postgrespro.ru>`, Postgres Professional, Moscow,
Russia

Alexander Korotkov `<a.korotkov@postgrespro.ru>`, Postgres Professional,
Moscow, Russia

Oleg Bartunov `<obartunov@postgrespro.ru>`, Postgres Professional,
Moscow,
Russia

</div>

</div>

<div class="navfooter">

-----

|                           |                    |                        |
| :------------------------ | :----------------: | ---------------------: |
| [Prev](auto-explain.html) | [Up](contrib.html) | [Next](btree-gin.html) |
| F.4. auto\_explain        | [Home](index.html) |        F.6. btree\_gin |

</div>
