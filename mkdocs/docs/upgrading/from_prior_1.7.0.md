# Upgrading from prior to MRBS 1.7.0

As a security measure, custom HTML for areas and rooms has been disabled by default, since it could be used to insert malicious JavaScript.  However, if you trust your admins you can re-enable it by setting the following in the config file:

```php-inline
$auth['allow_custom_html'] = true;
```