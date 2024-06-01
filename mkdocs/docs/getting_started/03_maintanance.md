# Maintaining the database

Be sure to back up your database regularly. For PostgreSQL, be sure to run the `vacuum` command regularly.

You can clean out old entries from your database using the supplied SQL scripts `purge.my.sql` (for MySQL) and `purge.pg.sql` (for PostgreSQL). Read the comments at the top of the scripts before using them.