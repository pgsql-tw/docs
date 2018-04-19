<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

67.1. BKI File Format

</div>

[Prev](bki.html "Chapter 67. BKI Backend Interface") 

[Up](bki.html "Chapter 67. BKI Backend Interface")

Chapter 67. BKI Backend Interface

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](bki-commands.html "67.2. BKI Commands")

-----

<div id="BKI-FORMAT" class="sect1">

<div class="titlepage">

<div>

<div>

## 67.1. BKI File Format

</div>

</div>

</div>

This section describes how the
<span class="productname">PostgreSQL</span> backend interprets BKI
files. This description will be easier to understand if the
`postgres.bki` file is at hand as an example.

BKI input consists of a sequence of commands. Commands are made up of a
number of tokens, depending on the syntax of the command. Tokens are
usually separated by whitespace, but need not be if there is no
ambiguity. There is no special command separator; the next token that
syntactically cannot belong to the preceding command starts a new one.
(Usually you would put a new command on a new line, for clarity.) Tokens
can be certain key words, special characters (parentheses, commas,
etc.), numbers, or double-quoted strings. Everything is case sensitive.

Lines starting with `#` are
ignored.

</div>

<div class="navfooter">

-----

|                                   |                    |                           |
| :-------------------------------- | :----------------: | ------------------------: |
| [Prev](bki.html)                  |   [Up](bki.html)   | [Next](bki-commands.html) |
| Chapter 67. BKI Backend Interface | [Home](index.html) |        67.2. BKI Commands |

</div>
