# POP3/IMAP
## POP3 Authentication
!!! info ""
    MRBS has POP3 Authentication support.  
    
This method will first try to authenticate using APOP, and fall back to standard USER/PASS if that fails. This can be used by setting `config.inc.php` as follows:

```php-inline
$auth["type"] = "pop3";
```

Also you will need to change the section:

```php-inline
// 'auth_pop3' configuration settings
$pop3_host = "pop3-server-name";    // Where is the POP3 server
$pop3_port = "110";                 // The POP3 server port
```

You will almost certainly not need to change the POP3 port number.

This method supports authentication against multiple servers using the syntax below:

=== "Multiple servers all using the same port"

    ```php-inline
    // 'auth_pop3' configuration settings
    // Where is the POP3 server
    $pop3_host[] = "localhost";
    $pop3_host[] = "myisp.co.uk";

    // The POP3 server port
    $pop3_port = "110";
    ```

=== "Multiple servers with the option of different ports for each server"

    ```php-inline
    // 'auth_pop3' configuration settings
    // Where is the POP3 server
    $pop3_host[] = "localhost";
    $pop3_host[] = "myisp.co.uk";

    // The POP3 server ports in the same order as the hosts
    $pop3_port[] = "110";
    $pop3_port[] = "110";
    ```

!!! note "Note"

    If you use the latter configuration then an equal number of hosts and ports must be specified or authentication will fail.

## IMAP Authentication
!!! info ""

    MRBS has IMAP Authentication support. Very similar in principle to the pop3 authentication.
    
This can be used by setting `config.inc.php` as follows:

```php-inline
$auth["type"] = "imap";
```

Also you will need to change the section:

```php-inline
//'auth_imap' configuration settings

// Where is the IMAP server
$imap_host = "imap-server-name";
// The IMAP server port
$imap_port = "143";
```

You will almost certainly not need to change the IMAP port number.

This method supports authentication against multiple servers using the syntax below.  It will try all the servers until a match is found or it fails to authenticate the user.

=== "Multiple servers all using the same port"

    ```php-inline
    # 'auth_imap' configuration settings
    # Where is the IMAP server
    $imap_host[] = "localhost";
    $imap_host[] = "myisp.co.uk";
    # The IMAP server port
    $imap_port = "143";
    ```

=== "Multiple servers with the option of different ports for each server"

    ```php-inline
    # 'auth_imap' configuration settings
    # Where is the IMAP server
    $imap_host[] = "localhost";
    $imap_host[] = "myisp.co.uk";
    # The IMAP server ports in the same order as the hosts
    $imap_port[] = "143";
    $imap_port[] = "143";
    ```

!!! note "Note"

    If you use the latter configuration then an equal number of hosts and ports must be specified or authentication will fail.
