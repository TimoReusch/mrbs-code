# Joomla!

Set in `config.inc.php`:

- 

    ```php-inline
    $auth['joomla']['rel_path'] = '..';   // Path to the Joomla! installation relative to MRBS.
    ```

!!! note "Note"
    Note that although in Joomla! access levels are solely used for what users are allowed to *see*, we use them in MRBS to determine what they can see and do, ie we map them onto MRBS user levels.  While this does not strictly follow the Joomla! access control model, it does make it much simpler to give users MRBS permissions.

- List of Joomla! viewing access level ids that have MRBS Admin capabilities.  You can if you wish use the existing viewing access levels.  However we recommend creating a new access level, e.g. "MRBS Administrator" and assigning that to user groups, as it will then be clearer which groups have what kind of access to MRBS:

    ```php-inline
    $auth['joomla']['admin_access_levels'] = array(); // Can either be a single integer, or an array of integers.
    ```

- As above, but for ordinary user rights.  Create for example a viewing access level called "MRBS User" and assign that level to user groups as appropriate.

    ```php-inline
    $auth['joomla']['user_access_levels'] = array(); // Can either be a single integer, or an array of integers.
    ```
