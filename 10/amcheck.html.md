<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

F.2. amcheck

</div>

[Prev](adminpack.html "F.1. adminpack") 

[Up](contrib.html "Appendix F. Additional Supplied Modules")

Appendix F. Additional Supplied Modules

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](auth-delay.html "F.3. auth_delay")

-----

<div id="AMCHECK" class="sect1">

<div class="titlepage">

<div>

<div>

## F.2. amcheck

</div>

</div>

</div>

<div class="toc">

<span class="sect2">[F.2.1.
Functions](amcheck.html#id-1.11.7.11.7)</span>

<span class="sect2">[F.2.2. Using `amcheck`
effectively](amcheck.html#id-1.11.7.11.8)</span>

<span class="sect2">[F.2.3. Repairing
corruption](amcheck.html#id-1.11.7.11.9)</span>

</div>

<span id="id-1.11.7.11.2" class="indexterm"></span>

The `amcheck` module provides functions that allow you to verify the
logical consistency of the structure of indexes. If the structure
appears to be valid, no error is raised.

The functions verify various <span class="emphasis">*invariants*</span>
in the structure of the representation of particular indexes. The
correctness of the access method functions behind index scans and other
important operations relies on these invariants always holding. For
example, certain functions verify, among other things, that all B-Tree
pages have items in
<span class="quote">“<span class="quote">logical</span>”</span> order
(e.g., for B-Tree indexes on `text`, index tuples should be in collated
lexical order). If that particular invariant somehow fails to hold, we
can expect binary searches on the affected page to incorrectly guide
index scans, resulting in wrong answers to SQL queries.

Verification is performed using the same procedures as those used by
index scans themselves, which may be user-defined operator class code.
For example, B-Tree index verification relies on comparisons made with
one or more B-Tree support function 1 routines. See
[Section 37.14.3](xindex.html#XINDEX-SUPPORT "37.14.3. Index Method Support Routines")
for details of operator class support functions.

`amcheck` functions may be used only by superusers.

<div id="id-1.11.7.11.7" class="sect2">

<div class="titlepage">

<div>

<div>

### F.2.1. Functions

</div>

</div>

</div>

<div class="variablelist">

  - <span class="term"> `bt_index_check(index regclass) returns void`
    <span id="id-1.11.7.11.7.2.1.1.2" class="indexterm"></span>
    </span>  
    `bt_index_check` tests that its target, a B-Tree index, respects a
    variety of invariants. Example usage:
    
    ``` screen
    test=# SELECT bt_index_check(c.oid), c.relname, c.relpages
    FROM pg_index i
    JOIN pg_opclass op ON i.indclass[0] = op.oid
    JOIN pg_am am ON op.opcmethod = am.oid
    JOIN pg_class c ON i.indexrelid = c.oid
    JOIN pg_namespace n ON c.relnamespace = n.oid
    WHERE am.amname = 'btree' AND n.nspname = 'pg_catalog'
    -- Don't check temp tables, which may be from another session:
    AND c.relpersistence != 't'
    -- Function may throw an error when this is omitted:
    AND i.indisready AND i.indisvalid
    ORDER BY c.relpages DESC LIMIT 10;
     bt_index_check |             relname             | relpages 
    ----------------+---------------------------------+----------
                    | pg_depend_reference_index       |       43
                    | pg_depend_depender_index        |       40
                    | pg_proc_proname_args_nsp_index  |       31
                    | pg_description_o_c_o_index      |       21
                    | pg_attribute_relid_attnam_index |       14
                    | pg_proc_oid_index               |       10
                    | pg_attribute_relid_attnum_index |        9
                    | pg_amproc_fam_proc_index        |        5
                    | pg_amop_opr_fam_index           |        5
                    | pg_amop_fam_strat_index         |        5
    (10 rows)
    ```
    
    This example shows a session that performs verification of every
    catalog index in the database
    <span class="quote">“<span class="quote">test</span>”</span>.
    Details of just the 10 largest indexes verified are displayed. Since
    no error is raised, all indexes tested appear to be logically
    consistent. Naturally, this query could easily be changed to call
    `bt_index_check` for every index in the database where verification
    is supported.
    
    `bt_index_check` acquires an `AccessShareLock` on the target index
    and the heap relation it belongs to. This lock mode is the same lock
    mode acquired on relations by simple `SELECT` statements.
    `bt_index_check` does not verify invariants that span child/parent
    relationships, nor does it verify that the target index is
    consistent with its heap relation. When a routine, lightweight test
    for corruption is required in a live production environment, using
    `bt_index_check` often provides the best trade-off between
    thoroughness of verification and limiting the impact on application
    performance and availability.

  - <span class="term"> `bt_index_parent_check(index regclass) returns
    void` <span id="id-1.11.7.11.7.2.2.1.2" class="indexterm"></span>
    </span>  
    `bt_index_parent_check` tests that its target, a B-Tree index,
    respects a variety of invariants. The checks performed by
    `bt_index_parent_check` are a superset of the checks performed by
    `bt_index_check`. `bt_index_parent_check` can be thought of as a
    more thorough variant of `bt_index_check`: unlike `bt_index_check`,
    `bt_index_parent_check` also checks invariants that span
    parent/child relationships. However, it does not verify that the
    target index is consistent with its heap relation.
    `bt_index_parent_check` follows the general convention of raising an
    error if it finds a logical inconsistency or other problem.
    
    A `ShareLock` is required on the target index by
    `bt_index_parent_check` (a `ShareLock` is also acquired on the heap
    relation). These locks prevent concurrent data modification from
    `INSERT`, `UPDATE`, and `DELETE` commands. The locks also prevent
    the underlying relation from being concurrently processed by
    `VACUUM`, as well as all other utility commands. Note that the
    function holds locks only while running, not for the entire
    transaction.
    
    `bt_index_parent_check`'s additional verification is more likely to
    detect various pathological cases. These cases may involve an
    incorrectly implemented B-Tree operator class used by the index that
    is checked, or, hypothetically, undiscovered bugs in the underlying
    B-Tree index access method code. Note that `bt_index_parent_check`
    cannot be used when Hot Standby mode is enabled (i.e., on read-only
    physical replicas), unlike `bt_index_check`.

</div>

</div>

<div id="id-1.11.7.11.8" class="sect2">

<div class="titlepage">

<div>

<div>

### F.2.2. Using `amcheck` effectively

</div>

</div>

</div>

`amcheck` can be effective at detecting various types of failure modes
that [<span class="application">data page
checksums</span>](app-initdb.html#APP-INITDB-DATA-CHECKSUMS) will always
fail to catch. These include:

<div class="itemizedlist">

  - Structural inconsistencies caused by incorrect operator class
    implementations.
    
    This includes issues caused by the comparison rules of operating
    system collations changing. Comparisons of datums of a collatable
    type like `text` must be immutable (just as all comparisons used for
    B-Tree index scans must be immutable), which implies that operating
    system collation rules must never change. Though rare, updates to
    operating system collation rules can cause these issues. More
    commonly, an inconsistency in the collation order between a master
    server and a standby server is implicated, possibly because the
    <span class="emphasis">*major*</span> operating system version in
    use is inconsistent. Such inconsistencies will generally only arise
    on standby servers, and so can generally only be detected on standby
    servers.
    
    If a problem like this arises, it may not affect each individual
    index that is ordered using an affected collation, simply because
    <span class="emphasis">*indexed*</span> values might happen to have
    the same absolute ordering regardless of the behavioral
    inconsistency. See
    [Section 23.1](locale.html "23.1. Locale Support") and
    [Section 23.2](collation.html "23.2. Collation Support") for
    further details about how
    <span class="productname">PostgreSQL</span> uses operating system
    locales and collations.

  - Corruption caused by hypothetical undiscovered bugs in the
    underlying <span class="productname">PostgreSQL</span> access method
    code or sort code.
    
    Automatic verification of the structural integrity of indexes plays
    a role in the general testing of new or proposed
    <span class="productname">PostgreSQL</span> features that could
    plausibly allow a logical inconsistency to be introduced. One
    obvious testing strategy is to call `amcheck` functions continuously
    when running the standard regression tests. See
    [Section 32.1](regress-run.html "32.1. Running the Tests") for
    details on running the tests.

  - File system or storage subsystem faults where checksums happen to
    simply not be enabled.
    
    Note that `amcheck` examines a page as represented in some shared
    memory buffer at the time of verification if there is only a shared
    buffer hit when accessing the block. Consequently, `amcheck` does
    not necessarily examine data read from the file system at the time
    of verification. Note that when checksums are enabled, `amcheck` may
    raise an error due to a checksum failure when a corrupt block is
    read into a buffer.

  - Corruption caused by faulty RAM, and the broader memory subsystem
    and operating system.
    
    <span class="productname">PostgreSQL</span> does not protect against
    correctable memory errors and it is assumed you will operate using
    RAM that uses industry standard Error Correcting Codes (ECC) or
    better protection. However, ECC memory is typically only immune to
    single-bit errors, and should not be assumed to provide
    <span class="emphasis">*absolute*</span> protection against failures
    that result in memory corruption.

</div>

In general, `amcheck` can only prove the presence of corruption; it
cannot prove its absence.

</div>

<div id="id-1.11.7.11.9" class="sect2">

<div class="titlepage">

<div>

<div>

### F.2.3. Repairing corruption

</div>

</div>

</div>

No error concerning corruption raised by `amcheck` should ever be a
false positive. In practice, `amcheck` is more likely to find software
bugs than problems with hardware. `amcheck` raises errors in the event
of conditions that, by definition, should never happen, and so careful
analysis of `amcheck` errors is often required.

There is no general method of repairing problems that `amcheck` detects.
An explanation for the root cause of an invariant violation should be
sought. [pageinspect](pageinspect.html "F.23. pageinspect") may play a
useful role in diagnosing corruption that `amcheck` detects. A `REINDEX`
may not be effective in repairing
corruption.

</div>

</div>

<div class="navfooter">

-----

|                        |                    |                         |
| :--------------------- | :----------------: | ----------------------: |
| [Prev](adminpack.html) | [Up](contrib.html) | [Next](auth-delay.html) |
| F.1. adminpack         | [Home](index.html) |        F.3. auth\_delay |

</div>
