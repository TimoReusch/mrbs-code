# Upgrading from prior to MRBS 1.6.0

If you upgrade to MRBS 1.6.0 and use your old config.inc.php file, you must add a line near to the top of the file, just after the `<?php`-tag, to make the file read:

```php
<?php
namespace MRBS;
```