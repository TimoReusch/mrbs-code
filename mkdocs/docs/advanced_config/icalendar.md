# iCalendar
- The default delimiter for separating the area and room in the LOCATION property of an iCalendar event. Note that no escaping of the delimiter is provided so it must not occur in room or area names.

    ```php-inline
    $default_area_room_delimiter = '/';
    ```

- Set the default source type for imports. Can be `file` or `url`

    ```php-inline
    $default_import_source = 'file';
    ```

- Default setting for importing past events

    ```php-inline
    $default_import_past = true;
    ```

- By default iCalendar notifications will be sent with the PARTSTAT property set to `NEEDS-ACTION`. If you set this variable to true then it will be set to `ACCEPTED`. This will change how the notification is treated by your email/calendar client. See [RFC 5545](https://datatracker.ietf.org/doc/html/rfc5545) for more details.

    ```php-inline
    $partstat_accepted = false;
    ```