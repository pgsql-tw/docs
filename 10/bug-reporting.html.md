<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

5. Bug Reporting Guidelines

</div>

[Prev](resources.html "4. Further Information") 

[Up](preface.html "Preface")

Preface

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](tutorial.html "Part I. Tutorial")

-----

<div id="BUG-REPORTING" class="sect1">

<div class="titlepage">

<div>

<div>

## 5. Bug Reporting Guidelines

</div>

</div>

</div>

<div class="toc">

<span class="sect2">[5.1. Identifying
Bugs](bug-reporting.html#id-1.3.8.5)</span>

<span class="sect2">[5.2. What to
Report](bug-reporting.html#id-1.3.8.6)</span>

<span class="sect2">[5.3. Where to Report
Bugs](bug-reporting.html#id-1.3.8.7)</span>

</div>

When you find a bug in <span class="productname">PostgreSQL</span> we
want to hear about it. Your bug reports play an important part in making
<span class="productname">PostgreSQL</span> more reliable because even
the utmost care cannot guarantee that every part of
<span class="productname">PostgreSQL</span> will work on every platform
under every circumstance.

The following suggestions are intended to assist you in forming bug
reports that can be handled in an effective fashion. No one is required
to follow them but doing so tends to be to everyone's advantage.

We cannot promise to fix every bug right away. If the bug is obvious,
critical, or affects a lot of users, chances are good that someone will
look into it. It could also happen that we tell you to update to a newer
version to see if the bug happens there. Or we might decide that the bug
cannot be fixed before some major rewrite we might be planning is done.
Or perhaps it is simply too hard and there are more important things on
the agenda. If you need help immediately, consider obtaining a
commercial support contract.

<div id="id-1.3.8.5" class="sect2">

<div class="titlepage">

<div>

<div>

### 5.1. Identifying Bugs

</div>

</div>

</div>

Before you report a bug, please read and re-read the documentation to
verify that you can really do whatever it is you are trying. If it is
not clear from the documentation whether you can do something or not,
please report that too; it is a bug in the documentation. If it turns
out that a program does something different from what the documentation
says, that is a bug. That might include, but is not limited to, the
following circumstances:

<div class="itemizedlist">

  - A program terminates with a fatal signal or an operating system
    error message that would point to a problem in the program. (A
    counterexample might be a
    <span class="quote">“<span class="quote">disk full</span>”</span>
    message, since you have to fix that yourself.)

  - A program produces the wrong output for any given input.

  - A program refuses to accept valid input (as defined in the
    documentation).

  - A program accepts invalid input without a notice or error message.
    But keep in mind that your idea of invalid input might be our idea
    of an extension or compatibility with traditional practice.

  - <span class="productname">PostgreSQL</span> fails to compile, build,
    or install according to the instructions on supported platforms.

</div>

Here <span class="quote">“<span class="quote">program</span>”</span>
refers to any executable, not only the backend process.

Being slow or resource-hogging is not necessarily a bug. Read the
documentation or ask on one of the mailing lists for help in tuning your
applications. Failing to comply to the SQL standard is not necessarily a
bug either, unless compliance for the specific feature is explicitly
claimed.

Before you continue, check on the TODO list and in the FAQ to see if
your bug is already known. If you cannot decode the information on the
TODO list, report your problem. The least we can do is make the TODO
list clearer.

</div>

<div id="id-1.3.8.6" class="sect2">

<div class="titlepage">

<div>

<div>

### 5.2. What to Report

</div>

</div>

</div>

The most important thing to remember about bug reporting is to state all
the facts and only facts. Do not speculate what you think went wrong,
what <span class="quote">“<span class="quote">it seemed to
do</span>”</span>, or which part of the program has a fault. If you
are not familiar with the implementation you would probably guess wrong
and not help us a bit. And even if you are, educated explanations are a
great supplement to but no substitute for facts. If we are going to fix
the bug we still have to see it happen for ourselves first. Reporting
the bare facts is relatively straightforward (you can probably copy and
paste them from the screen) but all too often important details are left
out because someone thought it does not matter or the report would be
understood anyway.

The following items should be contained in every bug report:

<div class="itemizedlist">

  - The exact sequence of steps <span class="emphasis">*from program
    start-up*</span> necessary to reproduce the problem. This should be
    self-contained; it is not enough to send in a bare `SELECT`
    statement without the preceding `CREATE TABLE` and `INSERT`
    statements, if the output should depend on the data in the tables.
    We do not have the time to reverse-engineer your database schema,
    and if we are supposed to make up our own data we would probably
    miss the problem.
    
    The best format for a test case for SQL-related problems is a file
    that can be run through the <span class="application">psql</span>
    frontend that shows the problem. (Be sure to not have anything in
    your `~/.psqlrc` start-up file.) An easy way to create this file is
    to use <span class="application">pg\_dump</span> to dump out the
    table declarations and data needed to set the scene, then add the
    problem query. You are encouraged to minimize the size of your
    example, but this is not absolutely necessary. If the bug is
    reproducible, we will find it either way.
    
    If your application uses some other client interface, such as
    <span class="application">PHP</span>, then please try to isolate the
    offending queries. We will probably not set up a web server to
    reproduce your problem. In any case remember to provide the exact
    input files; do not guess that the problem happens for
    <span class="quote">“<span class="quote">large files</span>”</span>
    or <span class="quote">“<span class="quote">midsize
    databases</span>”</span>, etc. since this information is too
    inexact to be of use.

  - The output you got. Please do not say that it
    <span class="quote">“<span class="quote">didn't
    work</span>”</span> or
    <span class="quote">“<span class="quote">crashed</span>”</span>.
    If there is an error message, show it, even if you do not understand
    it. If the program terminates with an operating system error, say
    which. If nothing at all happens, say so. Even if the result of your
    test case is a program crash or otherwise obvious it might not
    happen on our platform. The easiest thing is to copy the output from
    the terminal, if possible.
    
    <div class="note">
    
    ### Note
    
    If you are reporting an error message, please obtain the most
    verbose form of the message. In
    <span class="application">psql</span>, say `\set VERBOSITY verbose`
    beforehand. If you are extracting the message from the server log,
    set the run-time parameter
    [log\_error\_verbosity](runtime-config-logging.html#GUC-LOG-ERROR-VERBOSITY)
    to `verbose` so that all details are logged.
    
    </div>
    
    <div class="note">
    
    ### Note
    
    In case of fatal errors, the error message reported by the client
    might not contain all the information available. Please also look at
    the log output of the database server. If you do not keep your
    server's log output, this would be a good time to start doing so.
    
    </div>

  - The output you expected is very important to state. If you just
    write <span class="quote">“<span class="quote">This command gives me
    that output.</span>”</span> or
    <span class="quote">“<span class="quote">This is not what I
    expected.</span>”</span>, we might run it ourselves, scan the
    output, and think it looks OK and is exactly what we expected. We
    should not have to spend the time to decode the exact semantics
    behind your commands. Especially refrain from merely saying that
    <span class="quote">“<span class="quote">This is not what SQL
    says/Oracle does.</span>”</span> Digging out the correct behavior
    from SQL is not a fun undertaking, nor do we all know how all the
    other relational databases out there behave. (If your problem is a
    program crash, you can obviously omit this item.)

  - Any command line options and other start-up options, including any
    relevant environment variables or configuration files that you
    changed from the default. Again, please provide exact information.
    If you are using a prepackaged distribution that starts the database
    server at boot time, you should try to find out how that is done.

  - Anything you did at all differently from the installation
    instructions.

  - The <span class="productname">PostgreSQL</span> version. You can run
    the command `SELECT version();` to find out the version of the
    server you are connected to. Most executable programs also support a
    `--version` option; at least `postgres --version` and `psql
    --version` should work. If the function or the options do not exist
    then your version is more than old enough to warrant an upgrade. If
    you run a prepackaged version, such as RPMs, say so, including any
    subversion the package might have. If you are talking about a Git
    snapshot, mention that, including the commit hash.
    
    If your version is older than 10.3 we will almost certainly tell you
    to upgrade. There are many bug fixes and improvements in each new
    release, so it is quite possible that a bug you have encountered in
    an older release of <span class="productname">PostgreSQL</span> has
    already been fixed. We can only provide limited support for sites
    using older releases of <span class="productname">PostgreSQL</span>;
    if you require more than we can provide, consider acquiring a
    commercial support contract.

  - Platform information. This includes the kernel name and version, C
    library, processor, memory information, and so on. In most cases it
    is sufficient to report the vendor and version, but do not assume
    everyone knows what exactly
    <span class="quote">“<span class="quote">Debian</span>”</span>
    contains or that everyone runs on i386s. If you have installation
    problems then information about the toolchain on your machine
    (compiler, <span class="application">make</span>, and so on) is also
    necessary.

</div>

Do not be afraid if your bug report becomes rather lengthy. That is a
fact of life. It is better to report everything the first time than us
having to squeeze the facts out of you. On the other hand, if your input
files are huge, it is fair to ask first whether somebody is interested
in looking into it. Here is an
[article](http://www.chiark.greenend.org.uk/~sgtatham/bugs.html) that
outlines some more tips on reporting bugs.

Do not spend all your time to figure out which changes in the input make
the problem go away. This will probably not help solving it. If it turns
out that the bug cannot be fixed right away, you will still have time to
find and share your work-around. Also, once again, do not waste your
time guessing why the bug exists. We will find that out soon enough.

When writing a bug report, please avoid confusing terminology. The
software package in total is called
<span class="quote">“<span class="quote">PostgreSQL</span>”</span>,
sometimes
<span class="quote">“<span class="quote">Postgres</span>”</span> for
short. If you are specifically talking about the backend process,
mention that, do not just say
<span class="quote">“<span class="quote">PostgreSQL
crashes</span>”</span>. A crash of a single backend process is quite
different from crash of the parent
<span class="quote">“<span class="quote">postgres</span>”</span>
process; please don't say <span class="quote">“<span class="quote">the
server crashed</span>”</span> when you mean a single backend process
went down, nor vice versa. Also, client programs such as the interactive
frontend
<span class="quote">“<span class="quote"><span class="application">psql</span></span>”</span>
are completely separate from the backend. Please try to be specific
about whether the problem is on the client or server side.

</div>

<div id="id-1.3.8.7" class="sect2">

<div class="titlepage">

<div>

<div>

### 5.3. Where to Report Bugs

</div>

</div>

</div>

In general, send bug reports to the bug report mailing list at
`<pgsql-bugs@postgresql.org>`. You are requested to use a descriptive
subject for your email message, perhaps parts of the error message.

Another method is to fill in the bug report web-form available at the
project's [web site](https://www.postgresql.org/). Entering a bug report
this way causes it to be mailed to the `<pgsql-bugs@postgresql.org>`
mailing list.

If your bug report has security implications and you'd prefer that it
not become immediately visible in public archives, don't send it to
`pgsql-bugs`. Security issues can be reported privately to
`<security@postgresql.org>`.

Do not send bug reports to any of the user mailing lists, such as
`<pgsql-sql@postgresql.org>` or `<pgsql-general@postgresql.org>`. These
mailing lists are for answering user questions, and their subscribers
normally do not wish to receive bug reports. More importantly, they are
unlikely to fix them.

Also, please do <span class="emphasis">*not*</span> send reports to the
developers' mailing list `<pgsql-hackers@postgresql.org>`. This list is
for discussing the development of
<span class="productname">PostgreSQL</span>, and it would be nice if we
could keep the bug reports separate. We might choose to take up a
discussion about your bug report on `pgsql-hackers`, if the problem
needs more review.

If you have a problem with the documentation, the best place to report
it is the documentation mailing list `<pgsql-docs@postgresql.org>`.
Please be specific about what part of the documentation you are unhappy
with.

If your bug is a portability problem on a non-supported platform, send
mail to `<pgsql-hackers@postgresql.org>`, so we (and you) can work on
porting <span class="productname">PostgreSQL</span> to your platform.

<div class="note">

### Note

Due to the unfortunate amount of spam going around, all of the above
email addresses are closed mailing lists. That is, you need to be
subscribed to a list to be allowed to post on it. (You need not be
subscribed to use the bug-report web form, however.) If you would like
to send mail but do not want to receive list traffic, you can subscribe
and set your subscription option to `nomail`. For more information send
mail to `<majordomo@postgresql.org>` with the single word `help` in the
body of the message.

</div>

</div>

</div>

<div class="navfooter">

-----

|                        |                    |                       |
| :--------------------- | :----------------: | --------------------: |
| [Prev](resources.html) | [Up](preface.html) | [Next](tutorial.html) |
| 4. Further Information | [Home](index.html) |      Part I. Tutorial |

</div>
