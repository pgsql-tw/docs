<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

F.3. auth\_delay

</div>

[Prev](amcheck.html "F.2. amcheck") 

[Up](contrib.html "Appendix F. Additional Supplied Modules")

Appendix F. Additional Supplied Modules

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](auto-explain.html "F.4. auto_explain")

-----

<div id="AUTH-DELAY" class="sect1">

<div class="titlepage">

<div>

<div>

## F.3. auth\_delay

</div>

</div>

</div>

<div class="toc">

<span class="sect2">[F.3.1. Configuration
Parameters](auth-delay.html#id-1.11.7.12.5)</span>

<span class="sect2">[F.3.2.
Author](auth-delay.html#id-1.11.7.12.6)</span>

</div>

<span id="id-1.11.7.12.2" class="indexterm"></span>

`auth_delay` causes the server to pause briefly before reporting
authentication failure, to make brute-force attacks on database
passwords more difficult. Note that it does nothing to prevent
denial-of-service attacks, and may even exacerbate them, since processes
that are waiting before reporting authentication failure will still
consume connection slots.

In order to function, this module must be loaded via
[shared\_preload\_libraries](runtime-config-client.html#GUC-SHARED-PRELOAD-LIBRARIES)
in `postgresql.conf`.

<div id="id-1.11.7.12.5" class="sect2">

<div class="titlepage">

<div>

<div>

### F.3.1. Configuration Parameters

</div>

</div>

</div>

<div class="variablelist">

  - <span class="term"> `auth_delay.milliseconds` (`int`)
    <span id="id-1.11.7.12.5.2.1.1.3" class="indexterm"></span>
    </span>  
    The number of milliseconds to wait before reporting an
    authentication failure. The default is 0.

</div>

These parameters must be set in `postgresql.conf`. Typical usage might
be:

``` programlisting
# postgresql.conf
shared_preload_libraries = 'auth_delay'

auth_delay.milliseconds = '500'
```

</div>

<div id="id-1.11.7.12.6" class="sect2">

<div class="titlepage">

<div>

<div>

### F.3.2. Author

</div>

</div>

</div>

KaiGai Kohei
`<kaigai@ak.jp.nec.com>`

</div>

</div>

<div class="navfooter">

-----

|                      |                    |                           |
| :------------------- | :----------------: | ------------------------: |
| [Prev](amcheck.html) | [Up](contrib.html) | [Next](auto-explain.html) |
| F.2. amcheck         | [Home](index.html) |        F.4. auto\_explain |

</div>
