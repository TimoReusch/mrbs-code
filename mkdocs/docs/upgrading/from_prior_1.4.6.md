# Upgrading from prior MRBS 1.4.6

If you were previously using MRBS with `$unicode_encoding` set to 0, when you upgrade to 1.4.6 you **MUST** upgrade the MySQL database from the previously used character set to Unicode. Note that it is extremely
unlikely that you will need to do this as the default setting of `$unicode_encoding` is `1`.

If you do need to convert text in the database you should run the `convert_db_to_utf8.php` script **BEFORE** upgrading to the latest version of MRBS. The administrator should copy the file into the web directory, run it (choosing the encoding to convert from) ONCE, and then move it back out of the web directory. We recommend you backup your database before running this script if you are at all worried. Running it more than once will make a right mess of any non-ASCII text in the database.

Additionally, this script can correct an MRBS database that used to run on an old version of MySQL (earlier than 4.1), but that now runs on a newer version of MySQL. In this case, the database contains UTF-8 text, but the tables are considered to be in some other encoding by MySQL, generally Latin-1. The `convert_db_to_utf8.php` detects this condition, and offers the administrator the chance to correct the database 'collation'.

!!! abstract "Deprecated configuration variables"
    The following configuration variables are now deprecated. Their use is
    supported for the moment but you should change your config file now to
    use the new variables as support for the old variables may be dropped in the
    future:

    ```php-inline
    $mail_settings['admin_all']        replaced by  $mail_settings['on_new'] and
                                                    $mail_settings['on_change']
    $mail_settings['admin_on_delete']  replaced by  $mail_settings['on_delete']
    $dateformat                        replaced by  $strftime_format['daymonth']
    ```