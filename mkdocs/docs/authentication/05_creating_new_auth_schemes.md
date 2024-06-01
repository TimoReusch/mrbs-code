# Creating new authentication schemes
!!! info ""
    Authentication classes are responsible for validating user/password pairs. They must not attempt to communicate with the user.

Adding support for a new authentication service not yet supported by MRBS can be done using one of the two following techniques:

## Option 1: Adding a PHP class
You must create a new filed called `web/lib/MRBS/Auth/AuthXxx.php`.

It will be configured by setting in `config.inc.php`:

```php-inline
$auth["type"] = "xxx";
```

Underscores in the config variable need a `CamelCase`name. For example:

```php-inline
$auth["type"] = "xxx_yyy";
```

requires a file called `AuthXxxYyy.php`

The class should extend `\MRBS\Auth\Auth`.

## Option 2: Using an external program
External authentication programs are invoked via the internal PHP configuration proxy class called `AuthExt`.

The external program must take the username, password, and possibly other values as parameters. Its exit code must be a zero for "OK", and anything else for "not OK".

AuthExt takes the following parameters in `config.inc.php`:

```php-inline
$auth["type"] = "ext";

$auth["prog"] = "the_pathname_of_your_program";
$auth["params"] = "the arguments your program needs";
```

AuthExt constructs a command line to execute like:
```php-inline
$cmd = $auth["prog"] . ' ' . $auth["params"];

$cmd = preg_replace('/#USERNAME#/',$user,$cmd);
$cmd = preg_replace('/#PASSWORD#/',$pass,$cmd);
```

This should allow a lot of flexibility with different authenticators.