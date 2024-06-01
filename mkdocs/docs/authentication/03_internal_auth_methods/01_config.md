# `config`
!!! info ""
    Config Authentication uses no external server. It uses a list of users `config.inc.php`. Configure it as follows:

```php-inline
$auth["type"] = "config";
```

Then add the list of users:

```php-inline
$auth["user"]["username1"] = "password1";
$auth["user"]["username2"] = "password2";
// etc.
```

!!! note "Escape characters"

    If the password contains the characters `\` or `$`, then it's necessary to prefix them with a `\`. For example for password `pa$$word`, use the string `="pa\$\$word"`.

Finally put at least one of the users in the administrator's list:

```php-inline
$auth["admin"][] = "username2";
// etc.
```