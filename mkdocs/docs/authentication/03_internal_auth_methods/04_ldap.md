# LDAP

!!! info ""
    This method uses PHP's LDAP extension which must be compiled into PHP or installed and loaded as a plugin extension in your `php.ini`.

This method can be used by setting config.inc.php as follows:

```php-inline
$auth["type"] = "ldap";
```

Also you will need to change the section:

```php-inline
// 'auth_ldap' configuration settings
// Where is the LDAP server
$ldap_host = "localhost";

// If you have a non-standard LDAP port, you can define it here
//$ldap_port = 389;

// If you want to use LDAP v3, change the following to true
$ldap_v3 = false;

// If you want to use TLS, change following to true
$ldap_tls = false;

// LDAP base distinguish name
// See AUTHENTICATION for details of how check against multiple base dn's
$ldap_base_dn = "ou=organizationalunit,dc=my-domain,dc=com";

// Attribute within the base dn that contains the username
$ldap_user_attrib = "uid";
```

You only need to set `$ldap_port` if your LDAP server uses a non-standard port.

If you want to use LDAP v3, or TLS over LDAP set `$ldap_v3` or `$ldap_tls` to `true`, respectively.

This method will attempt an authenticated bind to the ldap server using the supplied password and a distinguished name, which is formed from the base distinguished name, the user attribute and the user name.

This method supports multiple values for most of the configuration parameters, including `$ldap_base_dn` entries and `$ldap_user_attrib` values. The authentication is attempted with each set of configuration parameters in turn until it succeeds or it fails to authenticate the user.

Multiple base distinguished names with the same user attribute for each base dn:
```php-inline
// 'auth_ldap' configuration settings
// Where is the LDAP server
$ldap_host = "localhost";

// LDAP base distinguish names
$ldap_base_dn[] = "ou=People, o=myCompany, c=US";
$ldap_base_dn[] = "ou=Administrators, o=myCompany, c=US";

$ldap_user_attrib = "uid";
```

Multiple base distinguished names with the option of different user attributes
for each base dn:
```php-inline
// 'auth_ldap' configuration settings
// Where is the LDAP server
$ldap_host = "localhost";

// LDAP base distinguish names
$ldap_base_dn[] = "ou=People, o=myCompany, c=US";
$ldap_base_dn[] = "ou=Administrators, o=myCompany, c=US";

$ldap_user_attrib[] = "uid";
$ldap_user_attrib[] = "cn";
```

!!! note "Note"

    If you use the latter configuration then an equal number of base dn's and user attributes must be specified or authentication will fail.
