# Installation Instructions
## Requirements
MRBS works with both MySQL (Version 5.5.3 and above, though MySQL Version 5.7.5 and above or MariaDB Version 10.0.2 and above are recommended) and PostgreSQL (Version 8.2 and above) systems.

> The reason that if you are using MySQL you are recommended to use MySQL Version 5.7.5 and above or MariaDB Version 10.0.2 and above is that these versions support multiple locks and thus enable MRBS to use a custom database session handler.  This is more secure than the standard file based session handler, easier to configure and enables MRBS to be used on clustered web servers.

- You must have at least PHP 7.2, with support for your chosen database system installed and working for this application. See the [PHP](www.php.net), [MySQL](www.mysql.com), and [PostgreSQL](www.postgresql.org) sites for more info on setting these up. You need to know how to install, secure, run, maintain, and back up your chosen database system.
- MRBS also requires the `iconv` PHP extension, to provide internationalisation.
- You can run PHP either as a CGI or with a direct module interface (also called SAPI). These servers include Apache, Microsoft Internet Information Server, Netscape and iPlanet servers. Many other servers have support for ISAPI, the Microsoft module interface.

    You'll get better performance with PHP setup as a module.  Not only will you not have to deal with the CGI performance hit, but you'll be able to use PHP's database connection pooling. However, be careful that you don't exceed the maximum number of connections allowed to your database; with connection pooling PHP/Apache can potentially create a connection from each Apache child server to the database.

    Also many MRBS authentication schemes use basic HTTP authentication. These don't work if you run PHP as a CGI.

    If you are using PHP as an Apache module, you probably want to ensure that the Apache MaxRequestsPerChild is not set to 0, in case of undetected memory leaks in PHP or MRBS.

## Getting started
The steps involved in installing MRBS are:

1.  Installing the MRBS files on your web server
2.  Creating the MRBS tables in your database
3.  Configuring MRBS

After that you can then point your browser at MRBS and start creating areas
and rooms.

### 1. Installing the MRBS files
To install MRBS, just unpack the distribution into a temporary directory, then copy the files in the `web` subdirectory into a new directory somewhere your web server can find them.

=== "Remote webserver"

    If you are using a remote webserver, unpack the files into a temporary directory on your local machine and then upload them to suitable directory on your webserver, for example `mydomain.com/mrbs`.

=== "vServer"

    If you are using a Unix/Linux webserver to which you have access, then an example of the installation might be:

    Unpack the software into a new temporary directory, something like this:
    ```bash
    $ tar -xvzf ~/download/mrbs-1.7.1.tgz       # (or whatever version)
    $ cd mrbs-1.7.1                             # (or whatever version)
    ```
    
    MRBS comes with a sample configuration file `web/config.inc.php-sample` - this should be copied to `web/config.inc.php` and configured for your site. The minimum you generally need to configure are the timezone (unless your PHP configuration already defines the timezone) and the database access details.
    
    If you are upgrading from a previous version of MRBS, you should consider copying your changes to the `config.inc.php` file into a new copy of `config.inc.php` copied from the new MRBS version's `config.inc.php-sample`.
    
    Now install MRBS by copying the contents of the `web` subdirectory of the
    distribution somewhere your web server can find it.  For example:
    
    ```bash
    $ cp -r web /var/lib/apache/htdocs/mrbs
    ```

### 2. Creating the MRBS tables in your database

=== "For a new install"

    !!! quote ""

        === "Remote webserver"
        
            If you are using a remote webserver you should use your database administration program (e.g. phpMyAdmin in your control panel) to create the MRBS tables, by executing the contents of `tables.my.sql`. 
            
            !!! example "Example"
            
                If you are using phpMyAdmin copy the contents of `tables.my.sql` into the SQL tab of phpMyAdmin and execute it as an SQL query. This assumes that you have already created a database - if not you should create a database first.
        
        === "vServer"
        
            If you are using a Unix/Linux webserver to which you have access, then the
            procedure might be:
            
            ```bash
            [MySQL]         $ mysqladmin create mrbs
            [PostgreSQL]    $ createdb -E UTF8 -T template0 mrbs
            ```
            
            (This will create a database named `mrbs`, but you can use any name.)
            
            Create the MRBS tables using the supplied `tables.*.sql` file:
            ```bash
            [MySQL]             $ mysql mrbs < tables.my.sql
            [PostgreSQL]        $ psql -a -f tables.pg.sql mrbs
            ```
            
            Where `mrbs` is the name of your database (mentioned above). This will create all the needed tables. 
            
            You may need to set rights on the tables; for PostgreSQL see `grant.pg.sql`. 
            
            If you need to delete the tables, for PostgreSQL see `destroy.pg.sql`.
            
            The tables are now empty and ready for use.

=== "For an upgrade"

    !!! quote ""

        If you are upgrading from MRBS 1.2-pre3 or later, your database will be upgraded automatically if necessary when you first run MRBS. You will be prompted for a database (not MRBS) username and password with rights to create and alter tables. Otherwise, please see the Upgrade section.
        
        **For a second installation or to use different table names**
        
        If you have table name conflicts or want to do a second installation and only have access to one database, then you can modify the `mrbs_`prefix for the table names.
        
        - In `tables.*.sql` you will need to change the table names and then follow the instructions above for creating the tables in your database.
          - When editing `config.inc.php`, you need to change the table name prefix from `mrbs_` to the value you chose using the variable `$db_tbl_prefix`.
        
        !!! danger "Warning"
        
            All of the `.sql` files are set up to use the `mrbs_` prefix therefore you will have to edit them before you use them if you change the prefix for your tables.

#### Extending the database
Please see the [Database Extension Section](02_database_extension.md) for details.

### 3. Configuring MRBS
Next, you will need to **create and customize** the file `config.inc.php`.

First of all, copy the file `config.inc.php-sample` to `config.inc.php`. Then you
need to edit the settings in that file and add other settings as required.

As a **minimum** you will need to set the **timezone** and the **database variables**. Other settings can be changed by copying lines from `systemdefaults.inc.php` and `areadefaults.inc.php` and pasting them into `config.inc.php`.

!!! danger "Important"

    Do not edit `systemdefaults.inc.php` or `areadefaults.inc.php`, as it will make it harder for you to upgrade when new versions are released.

Note that there are two different defaults files (`systemdefaults.inc.php` and `areadefaults.inc.php`) to draw attention to the fact that the settings in `areadefaults` just determine the settings for NEW areas. Settings for existing areas are set using a web browser by following the "Rooms" link in MRBS.  (It can be a little frustrating editing the area settings to find that they have no effect on existing areas!)

#### 3.1 Timezone
You must set the timezone to a valid value, a list of which can be found [here](http://php.net/manual/timezones.php). 

Don't forget to uncomment the line by removing the `//` at the beginning!

!!! note "Note"

    The timezone to use is the timezone in which your meeting rooms are located, not the timezone of your server (in case they are different).


??? tip "Existing bookings under a different timezone"

    If you already have bookings in your system which were made under a different timezone (perhaps if you are upgrading from a previous version of MRBS where the timezone wasn't set explicitly and the timezone defaulted tothat of the server) then you have two choices:

    1. Set the timezone to be the same as the previous timezone. This will ensure that all your existing bookings still appear correctly, but you will have to continue to put up with some minor inconveniences. For example,"Go to Today" will not always go to the today for your rooms, if you happen to be using MRBS at a time of day when the rooms are on one day but the timezone you have selected is on the day before or after.   Also if you are using the min and max book ahead facility then you will find that this is out by the difference between the timezone of your rooms and the timezone you have chosen.
    2. Set the timezone to the timezone of your rooms, having first corrected the start and end times of all your existing bookings, in both the entry and repeat tables. This is not a trivial exercise and you should back up your database before starting. Note also that it is not necessarily as simple as adding or subtracting a fixed number of hours to existing bookings since the dates at which your rooms change between summer and winter time may be different to the dates at which your previous timezone made the DST change. This can happen for example if your rooms are in Europe and your server is in the USA, as there is usually a week when Europe has changed but the USA has not.


#### 3.2 Database Settings
First, select your database system. Define one of the following:

```php-inline
$dbsys = "mysql";
$dbsys = "pgsql";
```

Then define your database connection parameters. Set the values for:

```php-inline
$db_host = …       // The hostname that the database server is running on.
$db_database = …   // The name of the database containing the MRBS tables.
$db_login = …      // The database login username
$db_password = …   // The database login password for the above login username
```

If the database server and web server are the same machine, use `$db_host="localhost"`. Or, with PostgreSQL only, you can use `$db_host=""` to use Unix Domain Sockets to connect to the database server on the same machine.

!!! abstract "Pooled connections"

    By default, MRBS will not use PHP persistent (pooled) database connections. Persistent connections can sometimes give better performance, but they can also cause problems with transactions and locks.  For more details see [here](http://php.net/manual/en/features.persistent-connections.php). 

    Although MRBS is designed to work with persistent connections we recommend that you don't use them unless they give a significant performance boost.  To use persistent connections set

    ```php-inline
    $db_persist = TRUE;
    ```
    
    If you want to install multiple sets of mrbs tables when only one SQL database is available, or resolve table name conflicts, you have to change the prefix of "mrbs_" for the tables in your database, then you will need to set the value of:

    ```php-inline
    $db_tbl_prefix = …    // The table name prefix
    ```

#### 3.3 Other Settings

Now go through `systemdefaults.inc.php` and `areadefaults.inc.php` and see which other configuration settings you would like to change.

Do this by copying them to `config.inc.php` and changing them there. This should make the task of upgrading to new releases easier as all your site-specific configuration changes will be confined to `config.inc.php`.

There is a wide variety of settings that can be changed, including

- site identification information
- themes
- calendar settings
- booking policies
- display and time format settings
- private bookings settings
- provisional bookings settings
- authentication settings
- email settings
- language settings
- report settings
- entry types

The comments in the `systemdefaults.inc.php` and `areadefaults.inc.php` files should explain the purpose of the various configuration variables and how to change them.

!!! note "Note"

    Some of the settings can be set on a per-area basis through the area administration page in MRBS. In this case the setting in the `areadefaults.inc.php` and `config.inc.php` files defines the default settings for new areas.)

The Help information is held in the site_faq files, one per language. You may well want to customise it by editing the files.

## Advanced configuration options
### Changing text strings
All the text strings used by MRBS, except those used on the Help page, are held in a series of `lang.*` files, for example `lang.en` for English, or `lang.fr` for French. They are overridden by the $vocab_override array in the config file.  If you want to change some of the text strings used by MRBS, for example change "Room" to "Computer", then look for the appropriate string in the lang files, remember its tag and set the `$vocab_override`-variable in the config-file. For example

```php-inline
$vocab_override['en']['ctrl_click'] = "Use Control-Click to select more than one computer";
```

Doing it this way, rather than editing the lang files, will mean that - when you upgrade to the next version of MRBS - you will not have to re-edit the `lang.*`-files.

### Internationalisation
From MRBS 1.4.6, MRBS is internationalised and uses UTF-8 throughout. MRBS will serve all of its pages in UTF-8 and stores everything in the database in UTF-8. This means that all languages work together.

For MRBS to use Unicode, PHP must be built with `iconv` support (`--with-iconv` directive), or have the iconv extension installed and enabled. On Windows, if you are using PHP 5, `iconv` support is built-in.


### Periods
When using periods, MRBS stores bookings internally as one minute slots starting at 1200. If once you have had your system up and running for a while find that you need to add a new period then you will need to adjust the times of your existing bookings unless your new period is at the end of the day.

The simplest example is when you want to add a new period at the start of the day. In this case you will need to run some SQL using phpMyAdmin or a similar program to move all bookings one minute later. In this example the SQL required would be

```sql
UPDATE mrbs_entry SET start_time=start_time+60, end_time=end_time+60;
UPDATE mrbs_repeat SET start_time=start_time+60, end_time=end_time+60,
end_date=end_date+60;
```

having changed `mrbs_` to your table prefix if necessary.

Before running this SQL, you should of course backup your database.


### Multisite
MRBS can be run in multisite mode by setting in the config-file:

```php-inline
$multisite = true;
```

In multisite mode a single instance of the MRBS code can serve a number of sites. Each site has its own config file in the `sites/<sitename>`-directory which will override any settings in the global config file.  At a minimum the local config file will contain a table prefix for the site, but can also contain any other config settings including a theme and custom CSS. The sites are reached by specifying the sitename in the query string, e.g. `index.php?site=sitename`.

!!! tip "Authentication in multiside mode"
    Individual sites can even have their own authentication methods, but note that authentication against WordPress multisite is not supported.


## Security Notes
You can configure your web server so that users can not obtain the `.inc`, files but this is not essential, since critical files containing your database login and password use a `.php` extension like `config.inc.php`. See your web server documentation on how to do this.

There are example Apache `.htaccess` files included, for different versions of Apache, but Apache might ignore a `.htaccess` file in your MRBS directory due to the setting of the "AllowOverride" directive in your web server configuration. Either change `AllowOverride None` to `AllowOverride Limit`, or create a new `<Directory>`-entry with the contents of the `.htaccess`-example-file in it for your MRBS installation. Then read the Apache docs five or six times, until you know what you just did.

You may protect `config.inc.php` to only allow the web server to read it. For example:

```
# chown httpd config.inc.php; 
chmod 400 config.inc.php
```

The script `testdata.php` is for testing only. Do not leave it in a directory accessible to your web server. Anyone running this will add a large number of test entries to your database, regardless of authentication, and book all your rooms to people you've never heard of.
