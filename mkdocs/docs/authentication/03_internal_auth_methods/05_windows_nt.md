# Windows NT
!!! info ""
    MRBS has Microsoft Windows NT/IIS authentication support. 
    
This can be used by setting `config.inc.php` as follows:

```php-inline
$auth["session"] = "nt";

$auth["type"] = "none";
```

!!! note "Note"

    Technically NT authentication is actually an MRBS session scheme. We put it in this list as it is related with authentication anyway.

To add an admin, just add his user's Windows user account like this:

```php-inline
$auth["admin"][] = "nt_username";
```

Also this scheme is fairly simple (it relies on IIS determining a user's Windows user account identity, before allowing the user to establish a network connection with the server) there are some requirements for it to work (these notes are based on IIS 5.0):

- You have to configure you mrbs directory (at least) to use either 'Basic Authentication' or 'Integrated Windows Authentication' (formerly called NTLM or Windows NT Challenge/Response authentication).

    ??? tip "Enable a WWW authentication method in IIS"

        In the Internet Information Services snap-in, select your site, but preferably only mrbs directory, and open its property sheets. Select the Directory Security property sheet. Under Anonymous Access and Authentication Control, click Edit. In the Authentication Methods dialog box, select one or more appropriate methods.

        The simplest way is to use 'Integrated Windows Authentication' because unlike 'Basic authentication', it does not prompt users for a user name and password. The current Windows user information on the client computer is used for the 'integrated Windows authentication'. It is also a more secure form of authentication because the user name and password are not sent across the network.

        But if you really want your users to enter their domain username/password, you have to select 'Basic Authentication'. The advantage of Basic authentication is that it is part of the HTTP specification, and is supported by most browsers. So, theoretically, you can log in with a valid domain username/password with a non-Microsoft browser, but frankly I did not tested it. The disadvantage is that 'Basic authentication' transmit passwords in an unencrypted form.

- To use 'Integrated Windows Authentication', you HAVE TO use the Microsoft browser too. Currently, only Internet Explorer, version 2.0 and later, supports this.
- The 'Anonymous access' must be disabled.
- If you enable both Basic and Integrated authentication, Integrated authentication takes precedence over Basic authentication.
- 'Integrated Windows authentication' does not work over HTTP Proxy connections or other firewall applications. So this scheme is more suitable for an Intranet, but 'Basic Authentication' should work (not tested).

All this was tested against IIS 5.0 on Windows 2000 server and Windows 2000 workstations using Internet Explorer 6.0, but this should work from IIS 4.0.
