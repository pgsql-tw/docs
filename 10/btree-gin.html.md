<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

F.6. btree\_gin

</div>

[Prev](bloom.html "F.5. bloom") 

[Up](contrib.html "Appendix F. Additional Supplied Modules")

Appendix F. Additional Supplied Modules

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](btree-gist.html "F.7. btree_gist")

-----

<div id="BTREE-GIN" class="sect1">

<div class="titlepage">

<div>

<div>

## F.6. btree\_gin

</div>

</div>

</div>

<div class="toc">

<span class="sect2">[F.6.1. Example
Usage](btree-gin.html#id-1.11.7.15.5)</span>

<span class="sect2">[F.6.2.
Authors](btree-gin.html#id-1.11.7.15.6)</span>

</div>

<span id="id-1.11.7.15.2" class="indexterm"></span>

`btree_gin` provides sample GIN operator classes that implement B-tree
equivalent behavior for the data types `int2`, `int4`, `int8`, `float4`,
`float8`, `timestamp with time zone`, `timestamp without time zone`,
`time with time zone`, `time without time zone`, `date`, `interval`,
`oid`, `money`, `"char"`, `varchar`, `text`, `bytea`, `bit`, `varbit`,
`macaddr`, `macaddr8`, `inet`, `cidr`, and all `enum` types.

In general, these operator classes will not outperform the equivalent
standard B-tree index methods, and they lack one major feature of the
standard B-tree code: the ability to enforce uniqueness. However, they
are useful for GIN testing and as a base for developing other GIN
operator classes. Also, for queries that test both a GIN-indexable
column and a B-tree-indexable column, it might be more efficient to
create a multicolumn GIN index that uses one of these operator classes
than to create two separate indexes that would have to be combined via
bitmap ANDing.

<div id="id-1.11.7.15.5" class="sect2">

<div class="titlepage">

<div>

<div>

### F.6.1. Example Usage

</div>

</div>

</div>

``` programlisting
CREATE TABLE test (a int4);
-- create index
CREATE INDEX testidx ON test USING GIN (a);
-- query
SELECT * FROM test WHERE a < 10;
```

</div>

<div id="id-1.11.7.15.6" class="sect2">

<div class="titlepage">

<div>

<div>

### F.6.2. Authors

</div>

</div>

</div>

Teodor Sigaev (`<teodor@stack.net>`) and Oleg Bartunov
(`<oleg@sai.msu.su>`). See
<http://www.sai.msu.su/~megera/oddmuse/index.cgi/Gin> for additional
information.

</div>

</div>

<div class="navfooter">

-----

|                    |                    |                         |
| :----------------- | :----------------: | ----------------------: |
| [Prev](bloom.html) | [Up](contrib.html) | [Next](btree-gist.html) |
| F.5. bloom         | [Home](index.html) |        F.7. btree\_gist |

</div>
