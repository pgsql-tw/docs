<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

20.2. User Name Maps

</div>

[Prev](auth-pg-hba-conf.html "20.1. The pg_hba.conf File") 

[Up](client-authentication.html "Chapter 20. Client Authentication")

Chapter 20. Client Authentication

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](auth-methods.html "20.3. Authentication Methods")

-----

<div id="AUTH-USERNAME-MAPS" class="sect1">

<div class="titlepage">

<div>

<div>

## 20.2. User Name Maps

</div>

</div>

</div>

<span id="id-1.6.7.9.2" class="indexterm"></span>

When using an external authentication system such as Ident or GSSAPI,
the name of the operating system user that initiated the connection
might not be the same as the database user (role) that is to be used. In
this case, a user name map can be applied to map the operating system
user name to a database user. To use user name mapping, specify
`map`=*`map-name`* in the options field in `pg_hba.conf`. This option is
supported for all authentication methods that receive external user
names. Since different mappings might be needed for different
connections, the name of the map to be used is specified in the
*`map-name`* parameter in `pg_hba.conf` to indicate which map to use for
each individual connection.

User name maps are defined in the ident map file, which by default is
named `pg_ident.conf`<span id="id-1.6.7.9.4.2" class="indexterm"></span>
and is stored in the cluster's data directory. (It is possible to place
the map file elsewhere, however; see the
[ident\_file](runtime-config-file-locations.html#GUC-IDENT-FILE)
configuration parameter.) The ident map file contains lines of the
general form:

``` synopsis
map-name system-username database-username
```

Comments and whitespace are handled in the same way as in `pg_hba.conf`.
The *`map-name`* is an arbitrary name that will be used to refer to this
mapping in `pg_hba.conf`. The other two fields specify an operating
system user name and a matching database user name. The same
*`map-name`* can be used repeatedly to specify multiple user-mappings
within a single map.

There is no restriction regarding how many database users a given
operating system user can correspond to, nor vice versa. Thus, entries
in a map should be thought of as meaning
<span class="quote">“<span class="quote">this operating system user is
allowed to connect as this database user</span>”</span>, rather than
implying that they are equivalent. The connection will be allowed if
there is any map entry that pairs the user name obtained from the
external authentication system with the database user name that the user
has requested to connect as.

If the *`system-username`* field starts with a slash (`/`), the
remainder of the field is treated as a regular expression. (See
[Section 9.7.3.1](functions-matching.html#POSIX-SYNTAX-DETAILS "9.7.3.1. Regular Expression Details")
for details of <span class="productname">PostgreSQL</span>'s regular
expression syntax.) The regular expression can include a single capture,
or parenthesized subexpression, which can then be referenced in the
*`database-username`* field as `\1` (backslash-one). This allows the
mapping of multiple user names in a single line, which is particularly
useful for simple syntax substitutions. For example, these entries

``` programlisting
mymap   /^(.*)@mydomain\.com$      \1
mymap   /^(.*)@otherdomain\.com$   guest
```

will remove the domain part for users with system user names that end
with `@mydomain.com`, and allow any user whose system name ends with
`@otherdomain.com` to log in as `guest`.

<div class="tip">

### Tip

Keep in mind that by default, a regular expression can match just part
of a string. It's usually wise to use `^` and `$`, as shown in the above
example, to force the match to be to the entire system user name.

</div>

The `pg_ident.conf` file is read on start-up and when the main server
process receives a
<span class="systemitem">SIGHUP</span><span id="id-1.6.7.9.8.3" class="indexterm"></span>
signal. If you edit the file on an active system, you will need to
signal the postmaster (using `pg_ctl reload` or `kill -HUP`) to make it
re-read the file.

A `pg_ident.conf` file that could be used in conjunction with the
`pg_hba.conf` file in
[Example 20.1](auth-pg-hba-conf.html#EXAMPLE-PG-HBA.CONF "Example 20.1. Example pg_hba.conf Entries")
is shown in
[Example 20.2](auth-username-maps.html#EXAMPLE-PG-IDENT.CONF "Example 20.2. An Example pg_ident.conf File").
In this example, anyone logged in to a machine on the 192.168 network
that does not have the operating system user name `bryanh`, `ann`, or
`robert` would not be granted access. Unix user `robert` would only be
allowed access when he tries to connect as
<span class="productname">PostgreSQL</span> user `bob`, not as `robert`
or anyone else. `ann` would only be allowed to connect as `ann`. User
`bryanh` would be allowed to connect as either `bryanh` or as `guest1`.

<div id="EXAMPLE-PG-IDENT.CONF" class="example">

**Example 20.2. An Example `pg_ident.conf` File**

<div class="example-contents">

``` programlisting
# MAPNAME       SYSTEM-USERNAME         PG-USERNAME

omicron         bryanh                  bryanh
omicron         ann                     ann
# bob has user name robert on these machines
omicron         robert                  bob
# bryanh can also connect as guest1
omicron         bryanh                  guest1
```

</div>

</div>

  

</div>

<div class="navfooter">

-----

|                               |                                  |                              |
| :---------------------------- | :------------------------------: | ---------------------------: |
| [Prev](auth-pg-hba-conf.html) | [Up](client-authentication.html) |    [Next](auth-methods.html) |
| 20.1. The `pg_hba.conf` File  |        [Home](index.html)        | 20.3. Authentication Methods |

</div>
