# Upgrading from prior to MRBS 1.5.0

MRBS's default authentication scheme changed from `config` to `db` with the release of MRBS 1.5.0. If you had previously used the 'config' scheme without specifically stating this in your config.inc.php you will need to make a change to your `config.inc.php` after upgrading to MRBS 1.5.0. The change you need is:

```php-inline
$auth["type"] = "config";
```