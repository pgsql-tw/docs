<div class="navheader" data-xmlns="http://www.w3.org/TR/xhtml1/transitional">

20.3. Authentication Methods

</div>

[Prev](auth-username-maps.html "20.2. User Name Maps") 

[Up](client-authentication.html "Chapter 20. Client Authentication")

Chapter 20. Client
Authentication

[Home](index.html "PostgreSQL 10.3 Documentation")

 [Next](client-authentication-problems.html "20.4. Authentication Problems")

-----

<div id="AUTH-METHODS" class="sect1">

<div class="titlepage">

<div>

<div>

## 20.3. Authentication Methods

</div>

</div>

</div>

<div class="toc">

<span class="sect2">[20.3.1. Trust
Authentication](auth-methods.html#AUTH-TRUST)</span>

<span class="sect2">[20.3.2. Password
Authentication](auth-methods.html#AUTH-PASSWORD)</span>

<span class="sect2">[20.3.3. GSSAPI
Authentication](auth-methods.html#GSSAPI-AUTH)</span>

<span class="sect2">[20.3.4. SSPI
Authentication](auth-methods.html#SSPI-AUTH)</span>

<span class="sect2">[20.3.5. Ident
Authentication](auth-methods.html#AUTH-IDENT)</span>

<span class="sect2">[20.3.6. Peer
Authentication](auth-methods.html#AUTH-PEER)</span>

<span class="sect2">[20.3.7. LDAP
Authentication](auth-methods.html#AUTH-LDAP)</span>

<span class="sect2">[20.3.8. RADIUS
Authentication](auth-methods.html#AUTH-RADIUS)</span>

<span class="sect2">[20.3.9. Certificate
Authentication](auth-methods.html#AUTH-CERT)</span>

<span class="sect2">[20.3.10. PAM
Authentication](auth-methods.html#AUTH-PAM)</span>

<span class="sect2">[20.3.11. BSD
Authentication](auth-methods.html#AUTH-BSD)</span>

</div>

The following subsections describe the authentication methods in more
detail.

<div id="AUTH-TRUST" class="sect2">

<div class="titlepage">

<div>

<div>

### 20.3.1. Trust Authentication

</div>

</div>

</div>

When `trust` authentication is specified,
<span class="productname">PostgreSQL</span> assumes that anyone who can
connect to the server is authorized to access the database with whatever
database user name they specify (even superuser names). Of course,
restrictions made in the `database` and `user` columns still apply. This
method should only be used when there is adequate operating-system-level
protection on connections to the server.

`trust` authentication is appropriate and very convenient for local
connections on a single-user workstation. It is usually
<span class="emphasis">*not*</span> appropriate by itself on a multiuser
machine. However, you might be able to use `trust` even on a multiuser
machine, if you restrict access to the server's Unix-domain socket file
using file-system permissions. To do this, set the
`unix_socket_permissions` (and possibly `unix_socket_group`)
configuration parameters as described in
[Section 19.3](runtime-config-connection.html "19.3. Connections and Authentication").
Or you could set the `unix_socket_directories` configuration parameter
to place the socket file in a suitably restricted directory.

Setting file-system permissions only helps for Unix-socket connections.
Local TCP/IP connections are not restricted by file-system permissions.
Therefore, if you want to use file-system permissions for local
security, remove the `host ... 127.0.0.1 ...` line from `pg_hba.conf`,
or change it to a non-`trust` authentication method.

`trust` authentication is only suitable for TCP/IP connections if you
trust every user on every machine that is allowed to connect to the
server by the `pg_hba.conf` lines that specify `trust`. It is seldom
reasonable to use `trust` for any TCP/IP connections other than those
from <span class="systemitem">localhost</span>
(127.0.0.1).

</div>

<div id="AUTH-PASSWORD" class="sect2">

<div class="titlepage">

<div>

<div>

### 20.3.2. Password Authentication

</div>

</div>

</div>

<span id="id-1.6.7.10.4.2" class="indexterm"></span><span id="id-1.6.7.10.4.3" class="indexterm"></span><span id="id-1.6.7.10.4.4" class="indexterm"></span>

There are several password-based authentication methods. These methods
operate similarly but differ in how the users' passwords are stored on
the server and how the password provided by a client is sent across the
connection.

<div class="variablelist">

  - <span class="term">`scram-sha-256`</span>  
    The method `scram-sha-256` performs SCRAM-SHA-256 authentication, as
    described in [RFC 7677](https://tools.ietf.org/html/rfc7677). It is
    a challenge-response scheme that prevents password sniffing on
    untrusted connections and supports storing passwords on the server
    in a cryptographically hashed form that is thought to be secure.
    
    This is the most secure of the currently provided methods, but it is
    not supported by older client libraries.

  - <span class="term">`md5`</span>  
    The method `md5` uses a custom less secure challenge-response
    mechanism. It prevents password sniffing and avoids storing
    passwords on the server in plain text but provides no protection if
    an attacker manages to steal the password hash from the server.
    Also, the MD5 hash algorithm is nowadays no longer considered secure
    against determined attacks.
    
    The `md5` method cannot be used with the
    [db\_user\_namespace](runtime-config-connection.html#GUC-DB-USER-NAMESPACE)
    feature.
    
    To ease transition from the `md5` method to the newer SCRAM method,
    if `md5` is specified as a method in `pg_hba.conf` but the user's
    password on the server is encrypted for SCRAM (see below), then
    SCRAM-based authentication will automatically be chosen instead.

  - <span class="term">`password`</span>  
    The method `password` sends the password in clear-text and is
    therefore vulnerable to password
    <span class="quote">“<span class="quote">sniffing</span>”</span>
    attacks. It should always be avoided if possible. If the connection
    is protected by SSL encryption then `password` can be used safely,
    though. (Though SSL certificate authentication might be a better
    choice if one is depending on using SSL).

</div>

<span class="productname">PostgreSQL</span> database passwords are
separate from operating system user passwords. The password for each
database user is stored in the `pg_authid` system catalog. Passwords can
be managed with the SQL commands [<span class="refentrytitle">CREATE
USER</span>](sql-createuser.html "CREATE USER") and
[<span class="refentrytitle">ALTER
ROLE</span>](sql-alterrole.html "ALTER ROLE"), e.g., **`CREATE USER foo
WITH PASSWORD 'secret'`**, or the <span class="application">psql</span>
command `\password`. If no password has been set up for a user, the
stored password is null and password authentication will always fail for
that user.

The availability of the different password-based authentication methods
depends on how a user's password on the server is encrypted (or hashed,
more accurately). This is controlled by the configuration parameter
[password\_encryption](runtime-config-connection.html#GUC-PASSWORD-ENCRYPTION)
at the time the password is set. If a password was encrypted using the
`scram-sha-256` setting, then it can be used for the authentication
methods `scram-sha-256` and `password` (but password transmission will
be in plain text in the latter case). The authentication method
specification `md5` will automatically switch to using the
`scram-sha-256` method in this case, as explained above, so it will also
work. If a password was encrypted using the `md5` setting, then it can
be used only for the `md5` and `password` authentication method
specifications (again, with the password transmitted in plain text in
the latter case). (Previous PostgreSQL releases supported storing the
password on the server in plain text. This is no longer possible.) To
check the currently stored password hashes, see the system catalog
`pg_authid`.

To upgrade an existing installation from `md5` to `scram-sha-256`, after
having ensured that all client libraries in use are new enough to
support SCRAM, set `password_encryption = 'scram-sha-256'` in
`postgresql.conf`, make all users set new passwords, and change the
authentication method specifications in `pg_hba.conf` to
`scram-sha-256`.

</div>

<div id="GSSAPI-AUTH" class="sect2">

<div class="titlepage">

<div>

<div>

### 20.3.3. GSSAPI Authentication

</div>

</div>

</div>

<span id="id-1.6.7.10.5.2" class="indexterm"></span>

<span class="productname">GSSAPI</span> is an industry-standard protocol
for secure authentication defined in RFC 2743.
<span class="productname">PostgreSQL</span> supports
<span class="productname">GSSAPI</span> with
<span class="productname">Kerberos</span> authentication according to
RFC 1964. <span class="productname">GSSAPI</span> provides automatic
authentication (single sign-on) for systems that support it. The
authentication itself is secure, but the data sent over the database
connection will be sent unencrypted unless SSL is used.

GSSAPI support has to be enabled when
<span class="productname">PostgreSQL</span> is built; see
[Chapter 16](installation.html "Chapter 16.  Installation from Source Code")
for more information.

When <span class="productname">GSSAPI</span> uses
<span class="productname">Kerberos</span>, it uses a standard principal
in the format `servicename`/*`hostname`*@*`realm`*. The PostgreSQL
server will accept any principal that is included in the keytab used by
the server, but care needs to be taken to specify the correct principal
details when making the connection from the client using the
`krbsrvname` connection parameter. (See also
[Section 33.1.2](libpq-connect.html#LIBPQ-PARAMKEYWORDS "33.1.2. Parameter Key Words").)
The installation default can be changed from the default `postgres` at
build time using `./configure --with-krb-srvnam=`*`whatever`*. In most
environments, this parameter never needs to be changed. Some Kerberos
implementations might require a different service name, such as
Microsoft Active Directory which requires the service name to be in
upper case (`POSTGRES`).

*`hostname`* is the fully qualified host name of the server machine. The
service principal's realm is the preferred realm of the server machine.

Client principals can be mapped to different
<span class="productname">PostgreSQL</span> database user names with
`pg_ident.conf`. For example, `pgusername@realm` could be mapped to just
`pgusername`. Alternatively, you can use the full `username@realm`
principal as the role name in
<span class="productname">PostgreSQL</span> without any mapping.

<span class="productname">PostgreSQL</span> also supports a parameter to
strip the realm from the principal. This method is supported for
backwards compatibility and is strongly discouraged as it is then
impossible to distinguish different users with the same user name but
coming from different realms. To enable this, set `include_realm` to 0.
For simple single-realm installations, doing that combined with setting
the `krb_realm` parameter (which checks that the principal's realm
matches exactly what is in the `krb_realm` parameter) is still secure;
but this is a less capable approach compared to specifying an explicit
mapping in `pg_ident.conf`.

Make sure that your server keytab file is readable (and preferably only
readable, not writable) by the
<span class="productname">PostgreSQL</span> server account. (See also
[Section 18.1](postgres-user.html "18.1. The PostgreSQL User Account").)
The location of the key file is specified by the
[krb\_server\_keyfile](runtime-config-connection.html#GUC-KRB-SERVER-KEYFILE)
configuration parameter. The default is
`/usr/local/pgsql/etc/krb5.keytab` (or whatever directory was specified
as `sysconfdir` at build time). For security reasons, it is recommended
to use a separate keytab just for the
<span class="productname">PostgreSQL</span> server rather than opening
up permissions on the system keytab file.

The keytab file is generated by the Kerberos software; see the Kerberos
documentation for details. The following example is for MIT-compatible
Kerberos 5 implementations:

``` screen
kadmin% ank -randkey postgres/server.my.domain.org
kadmin% ktadd -k krb5.keytab postgres/server.my.domain.org
```

When connecting to the database make sure you have a ticket for a
principal matching the requested database user name. For example, for
database user name `fred`, principal `fred@EXAMPLE.COM` would be able to
connect. To also allow principal `fred/users.example.com@EXAMPLE.COM`,
use a user name map, as described in
[Section 20.2](auth-username-maps.html "20.2. User Name Maps").

The following configuration options are supported for
<span class="productname">GSSAPI</span>:

<div class="variablelist">

  - <span class="term">`include_realm`</span>  
    If set to 0, the realm name from the authenticated user principal is
    stripped off before being passed through the user name mapping
    ([Section 20.2](auth-username-maps.html "20.2. User Name Maps")).
    This is discouraged and is primarily available for backwards
    compatibility, as it is not secure in multi-realm environments
    unless `krb_realm` is also used. It is recommended to leave
    `include_realm` set to the default (1) and to provide an explicit
    mapping in `pg_ident.conf` to convert principal names to
    <span class="productname">PostgreSQL</span> user names.

  - <span class="term">`map`</span>  
    Allows for mapping between system and database user names. See
    [Section 20.2](auth-username-maps.html "20.2. User Name Maps") for
    details. For a GSSAPI/Kerberos principal, such as
    `username@EXAMPLE.COM` (or, less commonly,
    `username/hostbased@EXAMPLE.COM`), the user name used for mapping is
    `username@EXAMPLE.COM` (or `username/hostbased@EXAMPLE.COM`,
    respectively), unless `include_realm` has been set to 0, in which
    case `username` (or `username/hostbased`) is what is seen as the
    system user name when mapping.

  - <span class="term">`krb_realm`</span>  
    Sets the realm to match user principal names against. If this
    parameter is set, only users of that realm will be accepted. If it
    is not set, users of any realm can connect, subject to whatever user
    name mapping is done.

</div>

</div>

<div id="SSPI-AUTH" class="sect2">

<div class="titlepage">

<div>

<div>

### 20.3.4. SSPI Authentication

</div>

</div>

</div>

<span id="id-1.6.7.10.6.2" class="indexterm"></span>

<span class="productname">SSPI</span> is a
<span class="productname">Windows</span> technology for secure
authentication with single sign-on.
<span class="productname">PostgreSQL</span> will use SSPI in `negotiate`
mode, which will use <span class="productname">Kerberos</span> when
possible and automatically fall back to
<span class="productname">NTLM</span> in other cases.
<span class="productname">SSPI</span> authentication only works when
both server and client are running
<span class="productname">Windows</span>, or, on non-Windows platforms,
when <span class="productname">GSSAPI</span> is available.

When using <span class="productname">Kerberos</span> authentication,
<span class="productname">SSPI</span> works the same way
<span class="productname">GSSAPI</span> does; see
[Section 20.3.3](auth-methods.html#GSSAPI-AUTH "20.3.3. GSSAPI Authentication")
for details.

The following configuration options are supported for
<span class="productname">SSPI</span>:

<div class="variablelist">

  - <span class="term">`include_realm`</span>  
    If set to 0, the realm name from the authenticated user principal is
    stripped off before being passed through the user name mapping
    ([Section 20.2](auth-username-maps.html "20.2. User Name Maps")).
    This is discouraged and is primarily available for backwards
    compatibility, as it is not secure in multi-realm environments
    unless `krb_realm` is also used. It is recommended to leave
    `include_realm` set to the default (1) and to provide an explicit
    mapping in `pg_ident.conf` to convert principal names to
    <span class="productname">PostgreSQL</span> user names.

  - <span class="term">`compat_realm`</span>  
    If set to 1, the domain's SAM-compatible name (also known as the
    NetBIOS name) is used for the `include_realm` option. This is the
    default. If set to 0, the true realm name from the Kerberos user
    principal name is used.
    
    Do not disable this option unless your server runs under a domain
    account (this includes virtual service accounts on a domain member
    system) and all clients authenticating through SSPI are also using
    domain accounts, or authentication will fail.

  - <span class="term">`upn_username`</span>  
    If this option is enabled along with `compat_realm`, the user name
    from the Kerberos UPN is used for authentication. If it is disabled
    (the default), the SAM-compatible user name is used. By default,
    these two names are identical for new user accounts.
    
    Note that <span class="application">libpq</span> uses the
    SAM-compatible name if no explicit user name is specified. If you
    use <span class="application">libpq</span> or a driver based on it,
    you should leave this option disabled or explicitly specify user
    name in the connection string.

  - <span class="term">`map`</span>  
    Allows for mapping between system and database user names. See
    [Section 20.2](auth-username-maps.html "20.2. User Name Maps") for
    details. For a SSPI/Kerberos principal, such as
    `username@EXAMPLE.COM` (or, less commonly,
    `username/hostbased@EXAMPLE.COM`), the user name used for mapping is
    `username@EXAMPLE.COM` (or `username/hostbased@EXAMPLE.COM`,
    respectively), unless `include_realm` has been set to 0, in which
    case `username` (or `username/hostbased`) is what is seen as the
    system user name when mapping.

  - <span class="term">`krb_realm`</span>  
    Sets the realm to match user principal names against. If this
    parameter is set, only users of that realm will be accepted. If it
    is not set, users of any realm can connect, subject to whatever user
    name mapping is done.

</div>

</div>

<div id="AUTH-IDENT" class="sect2">

<div class="titlepage">

<div>

<div>

### 20.3.5. Ident Authentication

</div>

</div>

</div>

<span id="id-1.6.7.10.7.2" class="indexterm"></span>

The ident authentication method works by obtaining the client's
operating system user name from an ident server and using it as the
allowed database user name (with an optional user name mapping). This is
only supported on TCP/IP connections.

<div class="note">

### Note

When ident is specified for a local (non-TCP/IP) connection, peer
authentication (see
[Section 20.3.6](auth-methods.html#AUTH-PEER "20.3.6. Peer Authentication"))
will be used instead.

</div>

The following configuration options are supported for
<span class="productname">ident</span>:

<div class="variablelist">

  - <span class="term">`map`</span>  
    Allows for mapping between system and database user names. See
    [Section 20.2](auth-username-maps.html "20.2. User Name Maps") for
    details.

</div>

The <span class="quote">“<span class="quote">Identification
Protocol</span>”</span> is described in RFC 1413. Virtually every
Unix-like operating system ships with an ident server that listens on
TCP port 113 by default. The basic functionality of an ident server is
to answer questions like <span class="quote">“<span class="quote">What
user initiated the connection that goes out of your port *`X`* and
connects to my port *`Y`*?</span>”</span>. Since
<span class="productname">PostgreSQL</span> knows both *`X`* and *`Y`*
when a physical connection is established, it can interrogate the ident
server on the host of the connecting client and can theoretically
determine the operating system user for any given connection.

The drawback of this procedure is that it depends on the integrity of
the client: if the client machine is untrusted or compromised, an
attacker could run just about any program on port 113 and return any
user name they choose. This authentication method is therefore only
appropriate for closed networks where each client machine is under tight
control and where the database and system administrators operate in
close contact. In other words, you must trust the machine running the
ident server. Heed the warning:

<div class="blockquote">

 

</div>

</div>

</div>

The Identification Protocol is not intended as an authorization or
access control protocol.

 

 

\--<span class="attribution">RFC 1413</span>

Some ident servers have a nonstandard option that causes the returned
user name to be encrypted, using a key that only the originating
machine's administrator knows. This option <span class="emphasis">*must
not*</span> be used when using the ident server with
<span class="productname">PostgreSQL</span>, since
<span class="productname">PostgreSQL</span> does not have any way to
decrypt the returned string to determine the actual user name.

<div id="AUTH-PEER" class="sect2">

<div class="titlepage">

<div>

<div>

### 20.3.6. Peer Authentication

</div>

</div>

</div>

<span id="id-1.6.7.10.8.2" class="indexterm"></span>

The peer authentication method works by obtaining the client's operating
system user name from the kernel and using it as the allowed database
user name (with optional user name mapping). This method is only
supported on local connections.

The following configuration options are supported for
<span class="productname">peer</span>:

<div class="variablelist">

  - <span class="term">`map`</span>  
    Allows for mapping between system and database user names. See
    [Section 20.2](auth-username-maps.html "20.2. User Name Maps") for
    details.

</div>

Peer authentication is only available on operating systems providing the
`getpeereid()` function, the `SO_PEERCRED` socket parameter, or similar
mechanisms. Currently that includes
<span class="systemitem">Linux</span>, most flavors of
<span class="systemitem">BSD</span> including
<span class="systemitem">macOS</span>, and
<span class="systemitem">Solaris</span>.

</div>

<div id="AUTH-LDAP" class="sect2">

<div class="titlepage">

<div>

<div>

### 20.3.7. LDAP Authentication

</div>

</div>

</div>

<span id="id-1.6.7.10.9.2" class="indexterm"></span>

This authentication method operates similarly to `password` except that
it uses LDAP as the password verification method. LDAP is used only to
validate the user name/password pairs. Therefore the user must already
exist in the database before LDAP can be used for authentication.

LDAP authentication can operate in two modes. In the first mode, which
we will call the simple bind mode, the server will bind to the
distinguished name constructed as *`prefix`* *`username`* *`suffix`*.
Typically, the *`prefix`* parameter is used to specify `cn=`, or
*`DOMAIN`*`\` in an Active Directory environment. *`suffix`* is used to
specify the remaining part of the DN in a non-Active Directory
environment.

In the second mode, which we will call the search+bind mode, the server
first binds to the LDAP directory with a fixed user name and password,
specified with *`ldapbinddn`* and *`ldapbindpasswd`*, and performs a
search for the user trying to log in to the database. If no user and
password is configured, an anonymous bind will be attempted to the
directory. The search will be performed over the subtree at
*`ldapbasedn`*, and will try to do an exact match of the attribute
specified in *`ldapsearchattribute`*. Once the user has been found in
this search, the server disconnects and re-binds to the directory as
this user, using the password specified by the client, to verify that
the login is correct. This mode is the same as that used by LDAP
authentication schemes in other software, such as Apache
`mod_authnz_ldap` and `pam_ldap`. This method allows for significantly
more flexibility in where the user objects are located in the directory,
but will cause two separate connections to the LDAP server to be made.

The following configuration options are used in both modes:

<div class="variablelist">

  - <span class="term">`ldapserver`</span>  
    Names or IP addresses of LDAP servers to connect to. Multiple
    servers may be specified, separated by spaces.

  - <span class="term">`ldapport`</span>  
    Port number on LDAP server to connect to. If no port is specified,
    the LDAP library's default port setting will be used.

  - <span class="term">`ldaptls`</span>  
    Set to 1 to make the connection between PostgreSQL and the LDAP
    server use TLS encryption. Note that this only encrypts the traffic
    to the LDAP server — the connection to the client will still be
    unencrypted unless SSL is used.

</div>

The following options are used in simple bind mode only:

<div class="variablelist">

  - <span class="term">`ldapprefix`</span>  
    String to prepend to the user name when forming the DN to bind as,
    when doing simple bind authentication.

  - <span class="term">`ldapsuffix`</span>  
    String to append to the user name when forming the DN to bind as,
    when doing simple bind authentication.

</div>

The following options are used in search+bind mode only:

<div class="variablelist">

  - <span class="term">`ldapbasedn`</span>  
    Root DN to begin the search for the user in, when doing search+bind
    authentication.

  - <span class="term">`ldapbinddn`</span>  
    DN of user to bind to the directory with to perform the search when
    doing search+bind authentication.

  - <span class="term">`ldapbindpasswd`</span>  
    Password for user to bind to the directory with to perform the
    search when doing search+bind authentication.

  - <span class="term">`ldapsearchattribute`</span>  
    Attribute to match against the user name in the search when doing
    search+bind authentication. If no attribute is specified, the `uid`
    attribute will be used.

  - <span class="term">`ldapurl`</span>  
    An RFC 4516 LDAP URL. This is an alternative way to write some of
    the other LDAP options in a more compact and standard form. The
    format is
    
    ``` synopsis
    ldap://host[:port]/basedn[?[attribute][?[scope]]]
    ```
    
    *`scope`* must be one of `base`, `one`, `sub`, typically the latter.
    Only one attribute is used, and some other components of standard
    LDAP URLs such as filters and extensions are not supported.
    
    For non-anonymous binds, `ldapbinddn` and `ldapbindpasswd` must be
    specified as separate options.
    
    To use encrypted LDAP connections, the `ldaptls` option has to be
    used in addition to `ldapurl`. The `ldaps` URL scheme (direct SSL
    connection) is not supported.
    
    LDAP URLs are currently only supported with OpenLDAP, not on
    Windows.

</div>

It is an error to mix configuration options for simple bind with options
for search+bind.

Here is an example for a simple-bind LDAP
configuration:

``` programlisting
host ... ldap ldapserver=ldap.example.net ldapprefix="cn=" ldapsuffix=", dc=example, dc=net"
```

When a connection to the database server as database user `someuser` is
requested, PostgreSQL will attempt to bind to the LDAP server using the
DN `cn=someuser, dc=example, dc=net` and the password provided by the
client. If that connection succeeds, the database access is granted.

Here is an example for a search+bind
configuration:

``` programlisting
host ... ldap ldapserver=ldap.example.net ldapbasedn="dc=example, dc=net" ldapsearchattribute=uid
```

When a connection to the database server as database user `someuser` is
requested, PostgreSQL will attempt to bind anonymously (since
`ldapbinddn` was not specified) to the LDAP server, perform a search for
`(uid=someuser)` under the specified base DN. If an entry is found, it
will then attempt to bind using that found information and the password
supplied by the client. If that second connection succeeds, the database
access is granted.

Here is the same search+bind configuration written as a
URL:

``` programlisting
host ... ldap ldapurl="ldap://ldap.example.net/dc=example,dc=net?uid?sub"
```

Some other software that supports authentication against LDAP uses the
same URL format, so it will be easier to share the configuration.

<div class="tip">

### Tip

Since LDAP often uses commas and spaces to separate the different parts
of a DN, it is often necessary to use double-quoted parameter values
when configuring LDAP options, as shown in the examples.

</div>

</div>

<div id="AUTH-RADIUS" class="sect2">

<div class="titlepage">

<div>

<div>

### 20.3.8. RADIUS Authentication

</div>

</div>

</div>

<span id="id-1.6.7.10.10.2" class="indexterm"></span>

This authentication method operates similarly to `password` except that
it uses RADIUS as the password verification method. RADIUS is used only
to validate the user name/password pairs. Therefore the user must
already exist in the database before RADIUS can be used for
authentication.

When using RADIUS authentication, an Access Request message will be sent
to the configured RADIUS server. This request will be of type
`Authenticate Only`, and include parameters for `user name`, `password`
(encrypted) and `NAS Identifier`. The request will be encrypted using a
secret shared with the server. The RADIUS server will respond to this
server with either `Access Accept` or `Access Reject`. There is no
support for RADIUS accounting.

Multiple RADIUS servers can be specified, in which case they will be
tried sequentially. If a negative response is received from a server,
the authentication will fail. If no response is received, the next
server in the list will be tried. To specify multiple servers, put the
names within quotes and separate the server names with a comma. If
multiple servers are specified, all other RADIUS options can also be
given as a comma separate list, to apply individual values to each
server. They can also be specified as a single value, in which case this
value will apply to all servers.

The following configuration options are supported for RADIUS:

<div class="variablelist">

  - <span class="term">`radiusservers`</span>  
    The name or IP addresses of the RADIUS servers to connect to. This
    parameter is required.

  - <span class="term">`radiussecrets`</span>  
    The shared secrets used when talking securely to the RADIUS server.
    This must have exactly the same value on the PostgreSQL and RADIUS
    servers. It is recommended that this be a string of at least 16
    characters. This parameter is required.
    
    <div class="note">
    
    ### Note
    
    The encryption vector used will only be cryptographically strong if
    <span class="productname">PostgreSQL</span> is built with support
    for <span class="productname">OpenSSL</span>. In other cases, the
    transmission to the RADIUS server should only be considered
    obfuscated, not secured, and external security measures should be
    applied if necessary.
    
    </div>

  - <span class="term">`radiusports`</span>  
    The port number on the RADIUS servers to connect to. If no port is
    specified, the default port `1812` will be used.

  - <span class="term">`radiusidentifiers`</span>  
    The string used as `NAS Identifier` in the RADIUS requests. This
    parameter can be used as a second parameter identifying for example
    which database user the user is attempting to authenticate as, which
    can be used for policy matching on the RADIUS server. If no
    identifier is specified, the default `postgresql` will be used.

</div>

</div>

<div id="AUTH-CERT" class="sect2">

<div class="titlepage">

<div>

<div>

### 20.3.9. Certificate Authentication

</div>

</div>

</div>

<span id="id-1.6.7.10.11.2" class="indexterm"></span>

This authentication method uses SSL client certificates to perform
authentication. It is therefore only available for SSL connections. When
using this authentication method, the server will require that the
client provide a valid, trusted certificate. No password prompt will be
sent to the client. The `cn` (Common Name) attribute of the certificate
will be compared to the requested database user name, and if they match
the login will be allowed. User name mapping can be used to allow `cn`
to be different from the database user name.

The following configuration options are supported for SSL certificate
authentication:

<div class="variablelist">

  - <span class="term">`map`</span>  
    Allows for mapping between system and database user names. See
    [Section 20.2](auth-username-maps.html "20.2. User Name Maps") for
    details.

</div>

In a `pg_hba.conf` record specifying certificate authentication, the
authentication option `clientcert` is assumed to be `1`, and it cannot
be turned off since a client certificate is necessary for this method.
What the `cert` method adds to the basic `clientcert` certificate
validity test is a check that the `cn` attribute matches the database
user name.

</div>

<div id="AUTH-PAM" class="sect2">

<div class="titlepage">

<div>

<div>

### 20.3.10. PAM Authentication

</div>

</div>

</div>

<span id="id-1.6.7.10.12.2" class="indexterm"></span>

This authentication method operates similarly to `password` except that
it uses PAM (Pluggable Authentication Modules) as the authentication
mechanism. The default PAM service name is `postgresql`. PAM is used
only to validate user name/password pairs and optionally the connected
remote host name or IP address. Therefore the user must already exist in
the database before PAM can be used for authentication. For more
information about PAM, please read the
[<span class="productname">Linux-PAM</span>
Page](http://www.kernel.org/pub/linux/libs/pam/).

The following configuration options are supported for PAM:

<div class="variablelist">

  - <span class="term">`pamservice`</span>  
    PAM service name.

  - <span class="term">`pam_use_hostname`</span>  
    Determines whether the remote IP address or the host name is
    provided to PAM modules through the `PAM_RHOST` item. By default,
    the IP address is used. Set this option to 1 to use the resolved
    host name instead. Host name resolution can lead to login delays.
    (Most PAM configurations don't use this information, so it is only
    necessary to consider this setting if a PAM configuration was
    specifically created to make use of it.)

</div>

<div class="note">

### Note

If PAM is set up to read `/etc/shadow`, authentication will fail because
the PostgreSQL server is started by a non-root user. However, this is
not an issue when PAM is configured to use LDAP or other authentication
methods.

</div>

</div>

<div id="AUTH-BSD" class="sect2">

<div class="titlepage">

<div>

<div>

### 20.3.11. BSD Authentication

</div>

</div>

</div>

<span id="id-1.6.7.10.13.2" class="indexterm"></span>

This authentication method operates similarly to `password` except that
it uses BSD Authentication to verify the password. BSD Authentication is
used only to validate user name/password pairs. Therefore the user's
role must already exist in the database before BSD Authentication can be
used for authentication. The BSD Authentication framework is currently
only available on OpenBSD.

BSD Authentication in <span class="productname">PostgreSQL</span> uses
the `auth-postgresql` login type and authenticates with the `postgresql`
login class if that's defined in `login.conf`. By default that login
class does not exist, and <span class="productname">PostgreSQL</span>
will use the default login class.

<div class="note">

### Note

To use BSD Authentication, the PostgreSQL user account (that is, the
operating system user running the server) must first be added to the
`auth` group. The `auth` group exists by default on OpenBSD
systems.

</div>

</div>

<div class="navfooter">

-----

|                                 |                                  |                                             |
| :------------------------------ | :------------------------------: | ------------------------------------------: |
| [Prev](auth-username-maps.html) | [Up](client-authentication.html) | [Next](client-authentication-problems.html) |
| 20.2. User Name Maps            |        [Home](index.html)        |               20.4. Authentication Problems |

</div>
