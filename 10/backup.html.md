<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

Chapter 25. Backup and Restore

</div>

[Prev](logfile-maintenance.html "24.3. Log File Maintenance") 

[Up](admin.html "Part III. Server Administration")

Part III. Server Administration

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](backup-dump.html "25.1. SQL Dump")

-----

<div id="BACKUP" class="chapter">

<div class="titlepage">

<div>

<div>

## Chapter 25. Backup and Restore

</div>

</div>

</div>

<div class="toc">

**Table of Contents**

<span class="sect1">[25.1. SQL Dump](backup-dump.html)</span>

<span class="sect2">[25.1.1. Restoring the
Dump](backup-dump.html#BACKUP-DUMP-RESTORE)</span>

<span class="sect2">[25.1.2. Using
<span class="application">pg\_dumpall</span>](backup-dump.html#BACKUP-DUMP-ALL)</span>

<span class="sect2">[25.1.3. Handling Large
Databases](backup-dump.html#BACKUP-DUMP-LARGE)</span>

<span class="sect1">[25.2. File System Level
Backup](backup-file.html)</span>

<span class="sect1">[25.3. Continuous Archiving and Point-in-Time
Recovery (PITR)](continuous-archiving.html)</span>

<span class="sect2">[25.3.1. Setting Up WAL
Archiving](continuous-archiving.html#BACKUP-ARCHIVING-WAL)</span>

<span class="sect2">[25.3.2. Making a Base
Backup](continuous-archiving.html#BACKUP-BASE-BACKUP)</span>

<span class="sect2">[25.3.3. Making a Base Backup Using the Low Level
API](continuous-archiving.html#BACKUP-LOWLEVEL-BASE-BACKUP)</span>

<span class="sect2">[25.3.4. Recovering Using a Continuous Archive
Backup](continuous-archiving.html#BACKUP-PITR-RECOVERY)</span>

<span class="sect2">[25.3.5.
Timelines](continuous-archiving.html#BACKUP-TIMELINES)</span>

<span class="sect2">[25.3.6. Tips and
Examples](continuous-archiving.html#BACKUP-TIPS)</span>

<span class="sect2">[25.3.7.
Caveats](continuous-archiving.html#CONTINUOUS-ARCHIVING-CAVEATS)</span>

</div>

<span id="id-1.6.12.2" class="indexterm"></span>

As with everything that contains valuable data,
<span class="productname">PostgreSQL</span> databases should be backed
up regularly. While the procedure is essentially simple, it is important
to have a clear understanding of the underlying techniques and
assumptions.

There are three fundamentally different approaches to backing up
<span class="productname">PostgreSQL</span> data:

<div class="itemizedlist">

  - SQL dump

  - File system level backup

  - Continuous archiving

</div>

Each has its own strengths and weaknesses; each is discussed in turn in
the following
sections.

</div>

<div class="navfooter">

-----

|                                  |                    |                          |
| :------------------------------- | :----------------: | -----------------------: |
| [Prev](logfile-maintenance.html) |  [Up](admin.html)  | [Next](backup-dump.html) |
| 24.3. Log File Maintenance       | [Home](index.html) |           25.1. SQL Dump |

</div>
