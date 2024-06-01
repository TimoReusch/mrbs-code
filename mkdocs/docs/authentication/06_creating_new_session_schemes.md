# Creating a new session scheme
!!! info ""
    Session classes manage the user interface for obtaining the user name and password. They also manage the way the name and password are recorded throughout a session.

To create a new scheme, you must create a new PHP class called `web/lib/MRBS/Session/SessionXxx.php`.

It will be configured by setting in `config.inc.php`:

```php-inline
$auth["session"] = "xxx";
```

Underscores in the config variable need a `CamelCase`-name. For example:

```php-inline
$auth["session"] = "xxx_yyy";
```

requires a file called `SessionXxxYyy.php`

The session class should extend either `\MRBS\Session\SessionWithLogin` or
`\MRBS\Session\SessionWithoutLogin`.

!!! note "Note"

    Session schemes are also useful in the case where a web server enforces its own session and authentication management. See the `SessionNt.php` and session `SessionOmni.php` files as examples. In this case, use in combination with authentication "none", as the real authentication is already done during the session initiation, and needs not be done again inside MRBS.