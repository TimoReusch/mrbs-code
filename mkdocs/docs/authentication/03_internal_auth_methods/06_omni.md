# OMNI

!!! info ""
    OMNI Authentication using Omnicron OmniHTTPd web server security features. Very similar in principle to NT authentication.

```php-inline
$auth["session"] = "omni";

$auth["type"] = "none";
```

!!! note "Note"
    Technically Omni authentication is actually an MRBS session scheme. We put it in this list as it is related with authentication anyway.