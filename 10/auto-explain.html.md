<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

F.4. auto\_explain

</div>

[Prev](auth-delay.html "F.3. auth_delay") 

[Up](contrib.html "Appendix F. Additional Supplied Modules")

Appendix F. Additional Supplied Modules

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](bloom.html "F.5. bloom")

-----

<div id="AUTO-EXPLAIN" class="sect1">

<div class="titlepage">

<div>

<div>

## F.4. auto\_explain

</div>

</div>

</div>

<div class="toc">

<span class="sect2">[F.4.1. Configuration
Parameters](auto-explain.html#id-1.11.7.13.5)</span>

<span class="sect2">[F.4.2.
Example](auto-explain.html#id-1.11.7.13.6)</span>

<span class="sect2">[F.4.3.
Author](auto-explain.html#id-1.11.7.13.7)</span>

</div>

<span id="id-1.11.7.13.2" class="indexterm"></span>

The `auto_explain` module provides a means for logging execution plans
of slow statements automatically, without having to run
[<span class="refentrytitle">EXPLAIN</span>](sql-explain.html "EXPLAIN")
by hand. This is especially helpful for tracking down un-optimized
queries in large applications.

The module provides no SQL-accessible functions. To use it, simply load
it into the server. You can load it into an individual session:

``` programlisting
LOAD 'auto_explain';
```

(You must be superuser to do that.) More typical usage is to preload it
into some or all sessions by including `auto_explain` in
[session\_preload\_libraries](runtime-config-client.html#GUC-SESSION-PRELOAD-LIBRARIES)
or
[shared\_preload\_libraries](runtime-config-client.html#GUC-SHARED-PRELOAD-LIBRARIES)
in `postgresql.conf`. Then you can track unexpectedly slow queries no
matter when they happen. Of course there is a price in overhead for
that.

<div id="id-1.11.7.13.5" class="sect2">

<div class="titlepage">

<div>

<div>

### F.4.1. Configuration Parameters

</div>

</div>

</div>

There are several configuration parameters that control the behavior of
`auto_explain`. Note that the default behavior is to do nothing, so you
must set at least `auto_explain.log_min_duration` if you want any
results.

<div class="variablelist">

  - <span class="term"> `auto_explain.log_min_duration` (`integer`)
    <span id="id-1.11.7.13.5.3.1.1.3" class="indexterm"></span>
    </span>  
    `auto_explain.log_min_duration` is the minimum statement execution
    time, in milliseconds, that will cause the statement's plan to be
    logged. Setting this to zero logs all plans. Minus-one (the default)
    disables logging of plans. For example, if you set it to `250ms`
    then all statements that run 250ms or longer will be logged. Only
    superusers can change this setting.

  - <span class="term"> `auto_explain.log_analyze` (`boolean`)
    <span id="id-1.11.7.13.5.3.2.1.3" class="indexterm"></span>
    </span>  
    `auto_explain.log_analyze` causes `EXPLAIN ANALYZE` output, rather
    than just `EXPLAIN` output, to be printed when an execution plan is
    logged. This parameter is off by default. Only superusers can change
    this setting.
    
    <div class="note">
    
    ### Note
    
    When this parameter is on, per-plan-node timing occurs for all
    statements executed, whether or not they run long enough to actually
    get logged. This can have an extremely negative impact on
    performance. Turning off `auto_explain.log_timing` ameliorates the
    performance cost, at the price of obtaining less information.
    
    </div>

  - <span class="term"> `auto_explain.log_buffers` (`boolean`)
    <span id="id-1.11.7.13.5.3.3.1.3" class="indexterm"></span>
    </span>  
    `auto_explain.log_buffers` controls whether buffer usage statistics
    are printed when an execution plan is logged; it's equivalent to the
    `BUFFERS` option of `EXPLAIN`. This parameter has no effect unless
    `auto_explain.log_analyze` is enabled. This parameter is off by
    default. Only superusers can change this setting.

  - <span class="term"> `auto_explain.log_timing` (`boolean`)
    <span id="id-1.11.7.13.5.3.4.1.3" class="indexterm"></span>
    </span>  
    `auto_explain.log_timing` controls whether per-node timing
    information is printed when an execution plan is logged; it's
    equivalent to the `TIMING` option of `EXPLAIN`. The overhead of
    repeatedly reading the system clock can slow down queries
    significantly on some systems, so it may be useful to set this
    parameter to off when only actual row counts, and not exact times,
    are needed. This parameter has no effect unless
    `auto_explain.log_analyze` is enabled. This parameter is on by
    default. Only superusers can change this setting.

  - <span class="term"> `auto_explain.log_triggers` (`boolean`)
    <span id="id-1.11.7.13.5.3.5.1.3" class="indexterm"></span>
    </span>  
    `auto_explain.log_triggers` causes trigger execution statistics to
    be included when an execution plan is logged. This parameter has no
    effect unless `auto_explain.log_analyze` is enabled. This parameter
    is off by default. Only superusers can change this setting.

  - <span class="term"> `auto_explain.log_verbose` (`boolean`)
    <span id="id-1.11.7.13.5.3.6.1.3" class="indexterm"></span>
    </span>  
    `auto_explain.log_verbose` controls whether verbose details are
    printed when an execution plan is logged; it's equivalent to the
    `VERBOSE` option of `EXPLAIN`. This parameter is off by default.
    Only superusers can change this setting.

  - <span class="term"> `auto_explain.log_format` (`enum`)
    <span id="id-1.11.7.13.5.3.7.1.3" class="indexterm"></span>
    </span>  
    `auto_explain.log_format` selects the `EXPLAIN` output format to be
    used. The allowed values are `text`, `xml`, `json`, and `yaml`. The
    default is text. Only superusers can change this setting.

  - <span class="term"> `auto_explain.log_nested_statements` (`boolean`)
    <span id="id-1.11.7.13.5.3.8.1.3" class="indexterm"></span>
    </span>  
    `auto_explain.log_nested_statements` causes nested statements
    (statements executed inside a function) to be considered for
    logging. When it is off, only top-level query plans are logged. This
    parameter is off by default. Only superusers can change this
    setting.

  - <span class="term"> `auto_explain.sample_rate` (`real`)
    <span id="id-1.11.7.13.5.3.9.1.3" class="indexterm"></span>
    </span>  
    `auto_explain.sample_rate` causes auto\_explain to only explain a
    fraction of the statements in each session. The default is 1,
    meaning explain all the queries. In case of nested statements,
    either all will be explained or none. Only superusers can change
    this setting.

</div>

In ordinary usage, these parameters are set in `postgresql.conf`,
although superusers can alter them on-the-fly within their own sessions.
Typical usage might be:

``` programlisting
# postgresql.conf
session_preload_libraries = 'auto_explain'

auto_explain.log_min_duration = '3s'
```

</div>

<div id="id-1.11.7.13.6" class="sect2">

<div class="titlepage">

<div>

<div>

### F.4.2. Example

</div>

</div>

</div>

``` programlisting
postgres=# LOAD 'auto_explain';
postgres=# SET auto_explain.log_min_duration = 0;
postgres=# SET auto_explain.log_analyze = true;
postgres=# SELECT count(*)
           FROM pg_class, pg_index
           WHERE oid = indrelid AND indisunique;
```

This might produce log output such as:

``` screen
LOG:  duration: 3.651 ms  plan:
  Query Text: SELECT count(*)
              FROM pg_class, pg_index
              WHERE oid = indrelid AND indisunique;
  Aggregate  (cost=16.79..16.80 rows=1 width=0) (actual time=3.626..3.627 rows=1 loops=1)
    ->  Hash Join  (cost=4.17..16.55 rows=92 width=0) (actual time=3.349..3.594 rows=92 loops=1)
          Hash Cond: (pg_class.oid = pg_index.indrelid)
          ->  Seq Scan on pg_class  (cost=0.00..9.55 rows=255 width=4) (actual time=0.016..0.140 rows=255 loops=1)
          ->  Hash  (cost=3.02..3.02 rows=92 width=4) (actual time=3.238..3.238 rows=92 loops=1)
                Buckets: 1024  Batches: 1  Memory Usage: 4kB
                ->  Seq Scan on pg_index  (cost=0.00..3.02 rows=92 width=4) (actual time=0.008..3.187 rows=92 loops=1)
                      Filter: indisunique
```

</div>

<div id="id-1.11.7.13.7" class="sect2">

<div class="titlepage">

<div>

<div>

### F.4.3. Author

</div>

</div>

</div>

Takahiro Itagaki `<itagaki.takahiro@oss.ntt.co.jp>`

</div>

</div>

<div class="navfooter">

-----

|                         |                    |                    |
| :---------------------- | :----------------: | -----------------: |
| [Prev](auth-delay.html) | [Up](contrib.html) | [Next](bloom.html) |
| F.3. auth\_delay        | [Home](index.html) |         F.5. bloom |

</div>
