<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

13.4. Data Consistency Checks at the Application Level

</div>

[Prev](explicit-locking.html "13.3. Explicit Locking") 

[Up](mvcc.html "Chapter 13. Concurrency Control")

Chapter 13. Concurrency Control

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](mvcc-caveats.html "13.5. Caveats")

-----

<div id="APPLEVEL-CONSISTENCY" class="sect1">

<div class="titlepage">

<div>

<div>

## 13.4. Data Consistency Checks at the Application Level

</div>

</div>

</div>

<div class="toc">

<span class="sect2">[13.4.1. Enforcing Consistency With Serializable
Transactions](applevel-consistency.html#SERIALIZABLE-CONSISTENCY)</span>

<span class="sect2">[13.4.2. Enforcing Consistency With Explicit
Blocking
Locks](applevel-consistency.html#NON-SERIALIZABLE-CONSISTENCY)</span>

</div>

It is very difficult to enforce business rules regarding data integrity
using Read Committed transactions because the view of the data is
shifting with each statement, and even a single statement may not
restrict itself to the statement's snapshot if a write conflict occurs.

While a Repeatable Read transaction has a stable view of the data
throughout its execution, there is a subtle issue with using MVCC
snapshots for data consistency checks, involving something known as
*read/write conflicts*. If one transaction writes data and a concurrent
transaction attempts to read the same data (whether before or after the
write), it cannot see the work of the other transaction. The reader then
appears to have executed first regardless of which started first or
which committed first. If that is as far as it goes, there is no
problem, but if the reader also writes data which is read by a
concurrent transaction there is now a transaction which appears to have
run before either of the previously mentioned transactions. If the
transaction which appears to have executed last actually commits first,
it is very easy for a cycle to appear in a graph of the order of
execution of the transactions. When such a cycle appears, integrity
checks will not work correctly without some help.

As mentioned in
[Section 13.2.3](transaction-iso.html#XACT-SERIALIZABLE "13.2.3. Serializable Isolation Level"),
Serializable transactions are just Repeatable Read transactions which
add nonblocking monitoring for dangerous patterns of read/write
conflicts. When a pattern is detected which could cause a cycle in the
apparent order of execution, one of the transactions involved is rolled
back to break the cycle.

<div id="SERIALIZABLE-CONSISTENCY" class="sect2">

<div class="titlepage">

<div>

<div>

### 13.4.1. Enforcing Consistency With Serializable Transactions

</div>

</div>

</div>

If the Serializable transaction isolation level is used for all writes
and for all reads which need a consistent view of the data, no other
effort is required to ensure consistency. Software from other
environments which is written to use serializable transactions to ensure
consistency should <span class="quote">“<span class="quote">just
work</span>”</span> in this regard in
<span class="productname">PostgreSQL</span>.

When using this technique, it will avoid creating an unnecessary burden
for application programmers if the application software goes through a
framework which automatically retries transactions which are rolled back
with a serialization failure. It may be a good idea to set
`default_transaction_isolation` to `serializable`. It would also be wise
to take some action to ensure that no other transaction isolation level
is used, either inadvertently or to subvert integrity checks, through
checks of the transaction isolation level in triggers.

See
[Section 13.2.3](transaction-iso.html#XACT-SERIALIZABLE "13.2.3. Serializable Isolation Level")
for performance suggestions.

<div class="warning">

### Warning

This level of integrity protection using Serializable transactions does
not yet extend to hot standby mode
([Section 26.5](hot-standby.html "26.5. Hot Standby")). Because of
that, those using hot standby may want to use Repeatable Read and
explicit locking on the master.

</div>

</div>

<div id="NON-SERIALIZABLE-CONSISTENCY" class="sect2">

<div class="titlepage">

<div>

<div>

### 13.4.2. Enforcing Consistency With Explicit Blocking Locks

</div>

</div>

</div>

When non-serializable writes are possible, to ensure the current
validity of a row and protect it against concurrent updates one must use
`SELECT FOR UPDATE`, `SELECT FOR SHARE`, or an appropriate `LOCK TABLE`
statement. (`SELECT FOR UPDATE` and `SELECT FOR SHARE` lock just the
returned rows against concurrent updates, while `LOCK TABLE` locks the
whole table.) This should be taken into account when porting
applications to <span class="productname">PostgreSQL</span> from other
environments.

Also of note to those converting from other environments is the fact
that `SELECT FOR UPDATE` does not ensure that a concurrent transaction
will not update or delete a selected row. To do that in
<span class="productname">PostgreSQL</span> you must actually update the
row, even if no values need to be changed. `SELECT FOR UPDATE`
<span class="emphasis">*temporarily blocks*</span> other transactions
from acquiring the same lock or executing an `UPDATE` or `DELETE` which
would affect the locked row, but once the transaction holding this lock
commits or rolls back, a blocked transaction will proceed with the
conflicting operation unless an actual `UPDATE` of the row was performed
while the lock was held.

Global validity checks require extra thought under non-serializable
MVCC. For example, a banking application might wish to check that the
sum of all credits in one table equals the sum of debits in another
table, when both tables are being actively updated. Comparing the
results of two successive `SELECT sum(...)` commands will not work
reliably in Read Committed mode, since the second query will likely
include the results of transactions not counted by the first. Doing the
two sums in a single repeatable read transaction will give an accurate
picture of only the effects of transactions that committed before the
repeatable read transaction started — but one might legitimately wonder
whether the answer is still relevant by the time it is delivered. If the
repeatable read transaction itself applied some changes before trying to
make the consistency check, the usefulness of the check becomes even
more debatable, since now it includes some but not all
post-transaction-start changes. In such cases a careful person might
wish to lock all tables needed for the check, in order to get an
indisputable picture of current reality. A `SHARE` mode (or higher) lock
guarantees that there are no uncommitted changes in the locked table,
other than those of the current transaction.

Note also that if one is relying on explicit locking to prevent
concurrent changes, one should either use Read Committed mode, or in
Repeatable Read mode be careful to obtain locks before performing
queries. A lock obtained by a repeatable read transaction guarantees
that no other transactions modifying the table are still running, but if
the snapshot seen by the transaction predates obtaining the lock, it
might predate some now-committed changes in the table. A repeatable read
transaction's snapshot is actually frozen at the start of its first
query or data-modification command (`SELECT`, `INSERT`, `UPDATE`, or
`DELETE`), so it is possible to obtain locks explicitly before the
snapshot is
frozen.

</div>

</div>

<div class="navfooter">

-----

|                               |                    |                           |
| :---------------------------- | :----------------: | ------------------------: |
| [Prev](explicit-locking.html) |  [Up](mvcc.html)   | [Next](mvcc-caveats.html) |
| 13.3. Explicit Locking        | [Home](index.html) |             13.5. Caveats |

</div>
