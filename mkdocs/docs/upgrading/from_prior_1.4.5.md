# Upgrading from prior MRBS 1.4.5

MRBS 1.4.5 introduces the concept of tentative bookings, or bookings that require confirmation. To avoid confusion, what were previously known as "provisional bookings" have now been renamed "bookings requiring approval" and the config variable `$provisional_enabled` has been renamed `$approval_enabled`. You should update your config file accordingly.

Please also see the note about database compatibility above.