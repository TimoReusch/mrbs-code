# Upgrade Information for previous releases of MRBS

If you are upgrading from MRBS 1.2-pre3 or later, MRBS will automatically execute any necessary database upgrades when it is first run. It will prompt you for a database (not MRBS) username and password with rights to create and alter tables.

It would be a sensible precaution to take a backup of your database before the upgrade.

1. Take a backup of your database, just in case.
2. Take a backup copy of your existing mrbs directory on your web server.
3. Upload all the files and directories, except the config file, in the web directory of the release to a new directory on your server. Copy your `config.inc.php` file from your old directory to the new one.  

    !!! note "Note"
        If you are upgrading from MRBS 1.4.7 or earlier, the structure of the config-file has changed and you should create a new config file based on `config.inc.php-sample`.

4. Go to MRBS in your browser. If a database upgrade is required, you'll be prompted for a database (note database, not MRBS) username and password.
5. Rename your directories so that the new one becomes the working one.

MRBS database upgrades are in general not backwards compatible, ie you won't be able to run an older version of MRBS against a later version of the database. You may therefore choose to make a copy of the database for test purposes and check that the upgrade process works before performing the upgrade on your production database.

See the advice in [Installation](../getting_started/01_installation.md) about potentially creating a fresh `config.inc.php` when you upgrade MRBS, especially for a major version change.










