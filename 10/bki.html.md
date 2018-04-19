<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

Chapter 67. BKI Backend Interface

</div>

[Prev](storage-page-layout.html "66.6. Database Page Layout") 

[Up](internals.html "Part VII. Internals")

Part VII. Internals

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](bki-format.html "67.1. BKI File Format")

-----

<div id="BKI" class="chapter">

<div class="titlepage">

<div>

<div>

## Chapter 67. BKI Backend Interface

</div>

</div>

</div>

<div class="toc">

**Table of Contents**

<span class="sect1">[67.1. BKI File Format](bki-format.html)</span>

<span class="sect1">[67.2. BKI Commands](bki-commands.html)</span>

<span class="sect1">[67.3. Structure of the Bootstrap BKI
File](bki-structure.html)</span>

<span class="sect1">[67.4. Example](bki-example.html)</span>

</div>

Backend Interface (BKI) files are scripts in a special language that is
understood by the <span class="productname">PostgreSQL</span> backend
when running in the
<span class="quote">“<span class="quote">bootstrap</span>”</span>
mode. The bootstrap mode allows system catalogs to be created and filled
from scratch, whereas ordinary SQL commands require the catalogs to
exist already. BKI files can therefore be used to create the database
system in the first place. (And they are probably not useful for
anything else.)

<span class="application">initdb</span> uses a BKI file to do part of
its job when creating a new database cluster. The input file used by
<span class="application">initdb</span> is created as part of building
and installing <span class="productname">PostgreSQL</span> by a program
named `genbki.pl`, which reads some specially formatted C header files
in the `src/include/catalog/` directory of the source tree. The created
BKI file is called `postgres.bki` and is normally installed in the
`share` subdirectory of the installation tree.

Related information can be found in the documentation for
<span class="application">initdb</span>.

</div>

<div class="navfooter">

-----

|                                  |                      |                         |
| :------------------------------- | :------------------: | ----------------------: |
| [Prev](storage-page-layout.html) | [Up](internals.html) | [Next](bki-format.html) |
| 66.6. Database Page Layout       |  [Home](index.html)  |   67.1. BKI File Format |

</div>
