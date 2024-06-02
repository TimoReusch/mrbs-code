# Entry Types
## Configuring booking types
- This array lists the configured entry type codes. The values map to a single char in the MRBS database, and so can be any permitted PHP array character.
The default descriptions of the entry types are held in the language files as `type.X` where `X` is the entry type. If you want to change the description you can override the default descriptions by setting the `$vocab_override`-config-variable.

    !!! example "Example"

        If you add a new booking type `C` the minimum you need to do is add a line to config.inc.php like:

        ```php-inline
        $vocab_override["en"]["type.C"] = "New booking type";
        ```

    Below is a basic default array which ensures there are at least some types defined.
    The proper type definitions should be made in `config.inc.php`. Each type has a color which is defined in the array `$color_types` in the `styling.inc`-file in the Themes directory

    ```php-inline
    unset($booking_types);    // Include this line when copying to config.inc.php
    $booking_types[] = "E";
    $booking_types[] = "I";
    ```

- If you don't want to use types then uncomment the following line.  (The booking will still have a type associated with it in the database, which will be the default type.)

    ```php-inline
    // unset($booking_types);
    ```

## Default descriptions
- Default brief description for new bookings

    ```php-inline
    $default_name = "";
    ```

- Set this to `true` if you want the booking name (brief description) to default to the current user's display name.  If set, this setting overrides `$default_name`.

    ```php-inline
    $default_name_display_name = false;
    ```

- Default long description for new bookings
    ```php-inline
    $default_description = "";
    ```