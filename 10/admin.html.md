<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

Part III. Server Administration

</div>

[Prev](parallel-safety.html "15.4. Parallel Safety") 

 

 

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](installation.html "Chapter 16.   Installation from Source Code")

-----

<div id="ADMIN" class="part">

<div class="titlepage">

<div>

<div>

</div>

</div>

</div>

<div id="id-1.6.2" class="partintro">

<div>

</div>

This part covers topics that are of interest to a
<span class="productname">PostgreSQL</span> database administrator. This
includes installation of the software, set up and configuration of the
server, management of users and databases, and maintenance tasks. Anyone
who runs a <span class="productname">PostgreSQL</span> server, even for
personal use, but especially in production, should be familiar with the
topics covered in this part.

The information in this part is arranged approximately in the order in
which a new user should read it. But the chapters are self-contained and
can be read individually as desired. The information in this part is
presented in a narrative fashion in topical units. Readers looking for a
complete description of a particular command should see
[Part VI](reference.html "Part VI. Reference").

The first few chapters are written so they can be understood without
prerequisite knowledge, so new users who need to set up their own server
can begin their exploration with this part. The rest of this part is
about tuning and management; that material assumes that the reader is
familiar with the general use of the
<span class="productname">PostgreSQL</span> database system. Readers are
encouraged to look at [Part I](tutorial.html "Part I. Tutorial") and
[Part II](sql.html "Part II. The SQL Language") for additional
information.

<div class="toc">

**Table of Contents**

<span class="chapter">[16. Installation from Source
Code](installation.html)</span>

<span class="sect1">[16.1. Short Version](install-short.html)</span>

<span class="sect1">[16.2.
Requirements](install-requirements.html)</span>

<span class="sect1">[16.3. Getting The
Source](install-getsource.html)</span>

<span class="sect1">[16.4. Installation
Procedure](install-procedure.html)</span>

<span class="sect1">[16.5. Post-Installation
Setup](install-post.html)</span>

<span class="sect1">[16.6. Supported
Platforms](supported-platforms.html)</span>

<span class="sect1">[16.7. Platform-specific
Notes](installation-platform-notes.html)</span>

<span class="chapter">[17. Installation from Source Code on
<span class="productname">Windows</span>](install-windows.html)</span>

<span class="sect1">[17.1. Building with
<span class="productname">Visual C++</span> or the
<span class="productname">Microsoft Windows
SDK</span>](install-windows-full.html)</span>

<span class="chapter">[18. Server Setup and
Operation](runtime.html)</span>

<span class="sect1">[18.1. The
<span class="productname">PostgreSQL</span> User
Account](postgres-user.html)</span>

<span class="sect1">[18.2. Creating a Database
Cluster](creating-cluster.html)</span>

<span class="sect1">[18.3. Starting the Database
Server](server-start.html)</span>

<span class="sect1">[18.4. Managing Kernel
Resources](kernel-resources.html)</span>

<span class="sect1">[18.5. Shutting Down the
Server](server-shutdown.html)</span>

<span class="sect1">[18.6. Upgrading a
<span class="productname">PostgreSQL</span>
Cluster](upgrading.html)</span>

<span class="sect1">[18.7. Preventing Server
Spoofing](preventing-server-spoofing.html)</span>

<span class="sect1">[18.8. Encryption
Options](encryption-options.html)</span>

<span class="sect1">[18.9. Secure TCP/IP Connections with
SSL](ssl-tcp.html)</span>

<span class="sect1">[18.10. Secure TCP/IP Connections with
<span class="application">SSH</span> Tunnels](ssh-tunnels.html)</span>

<span class="sect1">[18.11. Registering <span class="application">Event
Log</span> on
<span class="systemitem">Windows</span>](event-log-registration.html)</span>

<span class="chapter">[19. Server
Configuration](runtime-config.html)</span>

<span class="sect1">[19.1. Setting
Parameters](config-setting.html)</span>

<span class="sect1">[19.2. File
Locations](runtime-config-file-locations.html)</span>

<span class="sect1">[19.3. Connections and
Authentication](runtime-config-connection.html)</span>

<span class="sect1">[19.4. Resource
Consumption](runtime-config-resource.html)</span>

<span class="sect1">[19.5. Write Ahead
Log](runtime-config-wal.html)</span>

<span class="sect1">[19.6.
Replication](runtime-config-replication.html)</span>

<span class="sect1">[19.7. Query
Planning](runtime-config-query.html)</span>

<span class="sect1">[19.8. Error Reporting and
Logging](runtime-config-logging.html)</span>

<span class="sect1">[19.9. Run-time
Statistics](runtime-config-statistics.html)</span>

<span class="sect1">[19.10. Automatic
Vacuuming](runtime-config-autovacuum.html)</span>

<span class="sect1">[19.11. Client Connection
Defaults](runtime-config-client.html)</span>

<span class="sect1">[19.12. Lock
Management](runtime-config-locks.html)</span>

<span class="sect1">[19.13. Version and Platform
Compatibility](runtime-config-compatible.html)</span>

<span class="sect1">[19.14. Error
Handling](runtime-config-error-handling.html)</span>

<span class="sect1">[19.15. Preset
Options](runtime-config-preset.html)</span>

<span class="sect1">[19.16. Customized
Options](runtime-config-custom.html)</span>

<span class="sect1">[19.17. Developer
Options](runtime-config-developer.html)</span>

<span class="sect1">[19.18. Short
Options](runtime-config-short.html)</span>

<span class="chapter">[20. Client
Authentication](client-authentication.html)</span>

<span class="sect1">[20.1. The `pg_hba.conf`
File](auth-pg-hba-conf.html)</span>

<span class="sect1">[20.2. User Name
Maps](auth-username-maps.html)</span>

<span class="sect1">[20.3. Authentication
Methods](auth-methods.html)</span>

<span class="sect1">[20.4. Authentication
Problems](client-authentication-problems.html)</span>

<span class="chapter">[21. Database Roles](user-manag.html)</span>

<span class="sect1">[21.1. Database Roles](database-roles.html)</span>

<span class="sect1">[21.2. Role Attributes](role-attributes.html)</span>

<span class="sect1">[21.3. Role Membership](role-membership.html)</span>

<span class="sect1">[21.4. Dropping Roles](role-removal.html)</span>

<span class="sect1">[21.5. Default Roles](default-roles.html)</span>

<span class="sect1">[21.6. Function
Security](perm-functions.html)</span>

<span class="chapter">[22. Managing
Databases](managing-databases.html)</span>

<span class="sect1">[22.1. Overview](manage-ag-overview.html)</span>

<span class="sect1">[22.2. Creating a
Database](manage-ag-createdb.html)</span>

<span class="sect1">[22.3. Template
Databases](manage-ag-templatedbs.html)</span>

<span class="sect1">[22.4. Database
Configuration](manage-ag-config.html)</span>

<span class="sect1">[22.5. Destroying a
Database](manage-ag-dropdb.html)</span>

<span class="sect1">[22.6.
Tablespaces](manage-ag-tablespaces.html)</span>

<span class="chapter">[23. Localization](charset.html)</span>

<span class="sect1">[23.1. Locale Support](locale.html)</span>

<span class="sect1">[23.2. Collation Support](collation.html)</span>

<span class="sect1">[23.3. Character Set Support](multibyte.html)</span>

<span class="chapter">[24. Routine Database Maintenance
Tasks](maintenance.html)</span>

<span class="sect1">[24.1. Routine
Vacuuming](routine-vacuuming.html)</span>

<span class="sect1">[24.2. Routine
Reindexing](routine-reindex.html)</span>

<span class="sect1">[24.3. Log File
Maintenance](logfile-maintenance.html)</span>

<span class="chapter">[25. Backup and Restore](backup.html)</span>

<span class="sect1">[25.1. SQL Dump](backup-dump.html)</span>

<span class="sect1">[25.2. File System Level
Backup](backup-file.html)</span>

<span class="sect1">[25.3. Continuous Archiving and Point-in-Time
Recovery (PITR)](continuous-archiving.html)</span>

<span class="chapter">[26. High Availability, Load Balancing, and
Replication](high-availability.html)</span>

<span class="sect1">[26.1. Comparison of Different
Solutions](different-replication-solutions.html)</span>

<span class="sect1">[26.2. Log-Shipping Standby
Servers](warm-standby.html)</span>

<span class="sect1">[26.3. Failover](warm-standby-failover.html)</span>

<span class="sect1">[26.4. Alternative Method for Log
Shipping](log-shipping-alternative.html)</span>

<span class="sect1">[26.5. Hot Standby](hot-standby.html)</span>

<span class="chapter">[27. Recovery
Configuration](recovery-config.html)</span>

<span class="sect1">[27.1. Archive Recovery
Settings](archive-recovery-settings.html)</span>

<span class="sect1">[27.2. Recovery Target
Settings](recovery-target-settings.html)</span>

<span class="sect1">[27.3. Standby Server
Settings](standby-settings.html)</span>

<span class="chapter">[28. Monitoring Database
Activity](monitoring.html)</span>

<span class="sect1">[28.1. Standard Unix
Tools](monitoring-ps.html)</span>

<span class="sect1">[28.2. The Statistics
Collector](monitoring-stats.html)</span>

<span class="sect1">[28.3. Viewing Locks](monitoring-locks.html)</span>

<span class="sect1">[28.4. Progress
Reporting](progress-reporting.html)</span>

<span class="sect1">[28.5. Dynamic Tracing](dynamic-trace.html)</span>

<span class="chapter">[29. Monitoring Disk Usage](diskusage.html)</span>

<span class="sect1">[29.1. Determining Disk
Usage](disk-usage.html)</span>

<span class="sect1">[29.2. Disk Full Failure](disk-full.html)</span>

<span class="chapter">[30. Reliability and the Write-Ahead
Log](wal.html)</span>

<span class="sect1">[30.1. Reliability](wal-reliability.html)</span>

<span class="sect1">[30.2. Write-Ahead Logging
(WAL)](wal-intro.html)</span>

<span class="sect1">[30.3. Asynchronous
Commit](wal-async-commit.html)</span>

<span class="sect1">[30.4. WAL
Configuration](wal-configuration.html)</span>

<span class="sect1">[30.5. WAL Internals](wal-internals.html)</span>

<span class="chapter">[31. Logical
Replication](logical-replication.html)</span>

<span class="sect1">[31.1.
Publication](logical-replication-publication.html)</span>

<span class="sect1">[31.2.
Subscription](logical-replication-subscription.html)</span>

<span class="sect1">[31.3.
Conflicts](logical-replication-conflicts.html)</span>

<span class="sect1">[31.4.
Restrictions](logical-replication-restrictions.html)</span>

<span class="sect1">[31.5.
Architecture](logical-replication-architecture.html)</span>

<span class="sect1">[31.6.
Monitoring](logical-replication-monitoring.html)</span>

<span class="sect1">[31.7.
Security](logical-replication-security.html)</span>

<span class="sect1">[31.8. Configuration
Settings](logical-replication-config.html)</span>

<span class="sect1">[31.9. Quick
Setup](logical-replication-quick-setup.html)</span>

<span class="chapter">[32. Regression Tests](regress.html)</span>

<span class="sect1">[32.1. Running the Tests](regress-run.html)</span>

<span class="sect1">[32.2. Test
Evaluation](regress-evaluation.html)</span>

<span class="sect1">[32.3. Variant Comparison
Files](regress-variant.html)</span>

<span class="sect1">[32.4. TAP Tests](regress-tap.html)</span>

<span class="sect1">[32.5. Test Coverage
Examination](regress-coverage.html)</span>

</div>

</div>

</div>

<div class="navfooter">

-----

|                              |                    |                                            |
| :--------------------------- | :----------------: | -----------------------------------------: |
| [Prev](parallel-safety.html) |                    |                  [Next](installation.html) |
| 15.4. Parallel Safety        | [Home](index.html) | Chapter 16.  Installation from Source Code |

</div>
