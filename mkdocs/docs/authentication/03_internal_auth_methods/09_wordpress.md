# WordPress
Include the following snippets in the `config.inc.php`:

- 

    ```php-inline
    $auth['session'] = 'wordpress';
    $auth['type'] = 'wordpress';
    $auth['wordpress']['rel_path'] = '..';   // Path to the WordPress installation relative to MRBS.
    ```

- List of WordPress roles that have MRBS Admin capabilities.  The default is 'administrator'.

    !!! note "Note"
        These role names are the keys used to store the name, which are typically in lower case English, eg 'administrator', and not the values which are displayed on the dashboard form, which will generally start with a capital and be translated, eg 'Administrator' or 'Administrateur' (French), depending on the site language you have chosen for WordPress.

    You can define more than one WordPress role that maps to the MRBS Admin role by using an array.The comment below assumes that you have created a new WordPress role (probably by using a WordPress plugin) called "MRBS Admin", which will typically (depending on the plugin) have a key of 'mrbs_admin', and that you assigned that role to those users that you want to be MRBS admins.

    ```php-inline
    $auth['wordpress']['admin_roles'] = 'administrator';  // can also be an array, eg = array('administrator', 'mrbs_admin');
    ```

- List of WordPress roles that have MRBS User capabilities.  This allows you to have some WordPress users who are authorised to use MRBS and some who are not.

    ```php-inline
    $auth['wordpress']['user_roles'] = array('subscriber', 'contributor', 'author', 'editor', 'administrator');
    ```

Then add this to your WordPress `wp-config.php`-file:

```php-inline
// Define cookie paths so that login cookies can be shared with MRBS
$domain_name = 'example.com';  // Set to your domain name
define('COOKIEPATH', '/');
define('SITECOOKIEPATH', '/');

// In the definition below the '.' is necessary for older browsers (see
// http://php.net/manual/en/function.setcookie.php).
define('COOKIE_DOMAIN', ".$domain_name");
define('COOKIEHASH', md5($domain_name));
```