# Upgrading from prior to MRBS 1.8.0

The following configuration settings have changed:

| Config-Variable            | State      | Description                                      |
| -------------------------- | ---------- | ------------------------------------------------ |
| `$area_list_format`        | Redundant. | All area and room lists are now select elements. |
| `$display_calendar_bottom` | Redundant. | The mini-calendars have moved.                   |
| `$max_slots`               | Redundant. | The code has been rewritten.                     |
| `$simple_trailer`          | Redundant. | There is no longer a trailer!                    |



The following `$strftime_format`-settings have changed:

| Config-Variable | State       |                     |
| --------------- | ----------- | ------------------- |
| `day_month`     | Redundant.  |                     |
| `dayname_cal`   | replaced by | `minical_dayname`   |
| `month_cal`     | replaced by | `minical_monthname` |
| `monthyear`     | replaced by | `view_month`        |