# `REMOTE_USER`
!!! info ""
    Get user identity/password using the REMOTE_USER environment variable.

To use this session scheme, set in `config.inc.php`:

```php-inline
$auth['session']  = 'remote_user';
$auth['type'] = 'none';
```

If you want to display a logout link, set in `config.inc.php`:

```php-inline
$auth['remote_user']['logout_link'] = '/logout/link.html'
```