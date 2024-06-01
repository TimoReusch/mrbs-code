# How to configure
## Users

You don't configure users in MRBS. Users (and their password) are configured in
an external authentication server.

!!! info "Exceptions"

    - The "config" authentication is managed by MRBS, using a list of names and passwords in `config.inc.php`. This is done by specifying: 
    
        ```php-inline
        $auth["user"][ "username1"] = "password1"; 
        $auth["user"]["username2"] = "password2";
        // etc.
        ```
    
    - The "db" authentication is managed by MRBS, using a list of names and passwords stored in the MRBS database.

## Administrators
**Prerequisite**: Administrators are also users. They must have a valid user name
and password in the selected authentication server.

Administrators are defined in `config.inc.php`. This is done by specifying:

```php-inline
$auth["admin"][] = "username1";
$auth["admin"][] = "username2";
// etc.
```

You can have as many administrators as you want in this list.

!!! tip "Exceptions"
    
    In the "db" authentication method, access rights are stored in the MRBS database and are managed by MRBS.

The default MRBS configuration uses PHP sessions, and a user list in `config.inc.php` for authentication. It defines three demo users 'alice' 'bob' and 'administrator'. The 'administrator' user is also in the administrators list by default. You will almost surely want to change these.

If IP authentication is used, for "username" use the IP address. In this case, make sure to define the local host (127.0.0.1) as an administrator. This allows to conveniently administer the reservation system locally on the server.

## Session type
Choose one type in the list above, and set the $auth["session"] parameter in `config.inc.php`:

```php-inline
$auth["session"] = "php";
```

## Authentication type
Choose the internal authentication module you want.

## Internal PHP modules
If it's an internal PHP module, set the $auth["type"] parameter in `confing.inc.php`:

```php-inline
$auth["type"] = "pop3";
```

Then set the type-specific parameters in `config.inc.php`. See details in the authentication descriptions below.

## External programs

If you want to use an external authentication program, set `$auth["type"] = "ext";` and set the additional parameters:

- The name of the selected program in `$auth["prog"]`. Don't forget to prefix it with `./` under Unix and ensure that the program is executable by the `user` that the webserver runs as, e.g.

    ```php-inline
    $auth["prog"] = "./cryt_passwd.pl";
    ```

- The arguments to pass to the above program in `$auth["params"]`.

    ```php-inline
    $auth["params"] = "passwd_file #USERNAME# #PASSWORD#";
    ```

    At runtime `#USERNAME#` and `#PASSWORD#` will get replaced with the username and password that the user entered to login. The string can contain other arguments in addition to the above two.
