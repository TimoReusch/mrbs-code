# Extending the database
## General
It is possible to add extra columns to databbase tables, i.e. to the entry, repeat, room and users tables, if you need to hold extra information about bookings, rooms and users. For example, you might want to add a column in the room table to record whether a room has a coffee machine, and you might want to record the phone numbers of users. (Note that the users table is only used if you are using the `db` authentication scheme). Similarly, you might want to add a field to the entry and repeat tables to record the number of participants for a meeting.

To add extra columns, just go into your database administration tool, e.g. phpMyAdmin, and add the extra columns manually. MRBS will then recognise them
and handle them automatically, displaying the information in the lists of rooms and users and allowing you to edit the data in the appropriate forms.

!!! note "Notes"

    1. If you are adding a field to the entry table, you must add an identical field to the repeat table. If you do not, MRBS will fail with a fatal error when you try and run it.
    2. Names must consist of letters, numbers or underscores.  If you are using PostgreSQL then the name must begin with a letter or an underscore. If you are using MySQL then there is no restriction on the first character as long as it is in the permitted set, ie a letter, number or underscore.(Although MySQL will allow other characters in column names, MRBS imposes restrictions on the characters allowed in order to simplify the code. For a technical explanation see below).

At the moment only text, varchar, date (user table only), decimal/numeric, int, smallint and tinyint columns are  supported, displayed as textarea, text, date, number or checkbox fields as appropriate.  Whether a varchar is displayed as a text or textarea input depends on its maximum length, with the breakpoint determined by a configuration variable. Ints are treated as integer types, as you would expect. However, smallints and tinyints are assumed to be booleans and are displayed as checkboxes.

!!! note "Note"

    smallints are assumed to be booleans, because the boolean type in PostgreSQL presents some problems in PHP when trying to process the results of a query in a database independent way, so it is more convenient to use a smallint instead of a boolean in PostgreSQL.

Text descriptions are set in the `config`-file using the `$vocab_override`-variable, using the appropriate language(s) and with the tag `room.column_name`, (e.g. `room.coffee_machine`, or `users.phone`) enabling translations to be provided. If not present, the column name will be used for labels etc.

If you are adding columns to the entry and repeat tables then you only need to add the `entry.column_name`-tags: you don't need to add a `repeat.column_name`-tag.

!!! example "Example"

    To add a field to the room table recording whether or not there is a coffee machine, you would, in MySQL, add the column

    ```sql
    coffee_machine     tinyint
    ```

    to the room table and add the line

    ```php-inline
    $vocab_override['en']['room.coffee_machine'] = "Coffee machine";
    ```

    to the config file and similarly for other languages as required. MRBS should then do the rest and display your coffee machine field on the room pages.

## Extra options for custom fields
### Dropdown Boxes
You can create dropdown boxes for a custom field by defining an entry in the configuration array `#!php $select_options`. For example:

```php-inline
$select_options['entry.conference_facilities'] = array('Video',
                                                       'Telephone',
                                                       'None');
```

would define the `conference_facilities` custom field to have three possible values.

For custom fields only (will be extended later) it is also possible to use an associative array for $select_options, for example:

```php-inline
$select_options['entry.catering'] = array('c' => 'Coffee',
                                          's' => 'Sandwiches',
                                          'h' => 'Hot Lunch');
```

In this case the key (e.g. `'c'`) is stored in the database, but the value(e.g. `'Coffee'`) is displayed and can be searched for using Search and Report. This allows you to alter the displayed values, for example changing `'Coffee'` to `'Coffee, Tea and Biscuits'`, without having to alter the database. It can also be useful if the database table is being shared with another application. MRBS will auto-detect whether the array is associative.

### Mandatory fields
You can specify that a field is mandatory. This will ensure that the user specifies a value for a field that may be empty, like a text box or a selection, as  above. For example:

```php-inline
$is_mandatory_field['entry.conference_facilities'] = true;
```

This would define the `conference_facilities` custom field to be mandatory. In the case of a select field, this adds an empty value to the dropdown list.

Making a checkbox field mandatory is possible and requires the checkbox to be ticked before the form can be submitted. This can be useful for example for requiring users to accept terms of service or terms and conditions.

### Private fields
You can also specify that a field is private, ensuring that the contents are only visible to yourself and the administrators. For example:

```php-inline
$is_private_field['entry.refreshments'] = true;
$is_private_field['users.tel'] = true;
```

This would prevent the details of your refreshments being visible to other users - provided that private bookings are enabled. See the section on private bookings in `systemdefaults.inc.php` for more information.

Note that private fields are only supported in the entry and users tables.

### Regular Expressions

You can also enter regular expressions for validating text field input using the pattern attribute.  At the moment this is limited to custom fields in the users table. For example the following could be used to ensure a valid US ZIP code (you might want to have a better regex - this is just for illustration):

```php-inline
$pattern['users.zip_code'] = "^[0-9]{5}(?:-[0-9]{4})?$";
```

You would probably also want to enter a custom error message by using `$vocab_override`, with the tag consisting of `table.field.oninvalid` e.g.

```php-inline
$vocab_override['users.zip_code.oninvalid']['en'] = 
    "Please enter a valid ZIP code, e.g. '12345' or '12345-6789'";
```


## Technical explanation of the restriction on column names for custom fields
Column names for custom fields are used by MRBS in a number of ways:

- as the column name in the database
- as part of an HTML name attribute for a form input
- as part of a PHP variable name

MySQL, PostgreSQL, HTML and PHP all have different rules for these tokens:

- **MySQL**: almost anything is allowed except that:

    - "No identifier can contain ASCII NUL (0x00) or a byte with a value of 255."
    - "Database, table, and column names should not end with space characters." [Source](http://dev.mysql.com/doc/refman/5.0/en/identifiers.html)

- **PostgreSQL**: "SQL identifiers and key words must begin with a letter (a-z, but also letters with diacritical marks and non-Latin letters) or an underscore (_). Subsequent characters in an identifier or key word can be letters, underscores, digits (0-9), or dollar signs ($). Note that dollar signs are not allowed in identifiers according to the letter of the SQL standard, so their use may render applications less portable. The SQL standard will not define a key word that contains digits or starts or ends with an underscore, so identifiers of this form are safe against possible conflict with future extensions of the standard." [Source](http://www.postgresql.org/docs/8.1/interactive/sql-syntax.html#SQL-SYNTAX-IDENTIFIERS)
- PHP:  "Variable names follow the same rules as other labels in PHP. A valid variable name starts with a letter or underscore, followed by any number of letters, numbers, or underscores. As a regular expression, it would be expressed thus: `[a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*`" [Source](http://php.net/manual/en/language.variables.basics.php)
- HTML: "ID and NAME tokens must begin with a letter ([A-Za-z]) and may be followed by any number of letters, digits ([0-9]), hyphens ("-"), underscores ("_"), colons (":"), and periods (".")." [Source](http://www.w3.org/TR/html401/types.html#type-cdata)

In order to avoid complications with using names in each of these contexts, we restrict custom field names to the set of names which conforms to all four rules, taking into account the fact that when MRBS uses column names in PHP and HTML it always prefixes them with a string beginning with a letter. This gives us the rule that custom field names must consist of letters, numbers or underscores.
