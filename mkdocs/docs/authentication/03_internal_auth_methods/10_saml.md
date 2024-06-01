# SAML
Set in `config.inc.php`:

```php-inline
$auth['type'] = 'saml';
$auth['session'] = 'saml';
$auth['saml']['ssp_path'] = '/opt/simplesamlphp';
$auth['saml']['authsource'] = 'default-sp';
$auth['saml']['attr']['username'] = 'sAMAccountName';
$auth['saml']['attr']['mail'] = 'mail';
$auth['saml']['admin']['memberOf'] = ['CN=Domain Admins,CN=Users,DC=example,DC=com'];

// Optional access control filter
$auth['saml']['user']['memberOf'] = ['CN=Calendar Users,CN=Users,DC=example,DC=com'];
```

This scheme assumes that you've already configured SimpleSamlPhp, that
you have configured oid2name mapping, and that you have set up aliases
in your webserver so that SimpleSamlPhp can handle incoming assertions.
Refer to the SimpleSamlPhp documentation for more information on how to
do that:

- [https://simplesamlphp.org/docs/stable/simplesamlphp-install](https://simplesamlphp.org/docs/stable/simplesamlphp-install)
- [https://simplesamlphp.org/docs/stable/simplesamlphp-sp](https://simplesamlphp.org/docs/stable/simplesamlphp-sp)

