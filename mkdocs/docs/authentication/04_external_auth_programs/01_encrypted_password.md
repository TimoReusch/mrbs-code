# Encrypted password authentication
`crypt_passwd.pl` is a short Perl script, which can be used to demonstratehow the "ext" authentication provider works.

??? abstract "crypt_passwd.pl"

    ```bash
    #!/usr/bin/perl

    # Authentication script to use with MRBS's "ext" authentication
    # scheme. config.inc.php should include something like:
    #
    # $auth["realm"]  = "MRBS";
    # $auth["type"]   = "ext";
    # $auth["prog"]   = "../crypt_passwd.pl";
    # $auth["params"] = "/etc/httpd/mrbs_passwd #USERNAME# #PASSWORD#";
    #
    # The script takes 3 pararameters:
    #
    # PASSWDFILE USERNAME PASSWORD
    #
    # Where:
    #
    # PASSWDFILE - Filename of password file, which is the form
    #              <username>:<crypted password>
    #              [See crypt_passwd.example for an example]
    #              You should make sure this is readable by the
    #              user that PHP (most likely the web server)
    #              runs as.
    # USERNAME   - Username to check
    # PASSWORD   - Password to check against crypted password in
    #              password file
    #
    # Returns 0 on success, 1 on failure

    use strict;
    use warnings;

    my $passwd_filename = shift || die "No passwd filename supplied\n";
    my $username = shift || die "No username supplied\n";
    my $password = shift || die "No password supplied\n";

    my $retcode = 1;

    open PASSWD,'<',$passwd_filename;

    while (<PASSWD>)
    {
      if (m/^([^:]+):(.*)$/)
      {
        my $user = $1;
        my $crypt = $2;
  
        if ($user eq $username)
        {
          if (crypt($password, $crypt) eq $crypt)
          {
            $retcode = 0;
            last;
          }
          else
          {
              last;
          }
        }
      }
    }
    close PASSWD;

    exit $retcode;
    ```

It utilises uses a file containing usernames and their encrypted passwords.

`config.inc.php` should be changed to have a section that reads something like:

```php-inline
$auth["type"] = "ext";

$auth["prog"] = "../crypt_passwd.pl";
$auth["params"] = "/etc/httpd/mrbs_passwd #USERNAME# #PASSWORD#";
```

As you can see the `crypt_passwd.pl` script takes 3 parameters:

- The first parameter is the filename of the password file. 

    It must be readable by the user which PHP is running as, which is usually the user the web server is running as. The password file must consist of pairs of username and encrypted password, separated by a colon. See `crypt_passwd.example` for an example password file.

- Username
- Password

??? abstract "crypt_passwd.example"

    ```bash
    # SHA-512 crypted passwords, for better security
    user1:$6$FGcixVeC$J6Vb/7PKwcOn3M9ma2n4PtFx.83bA5p9s1Qk/GYOHgGZMw1rpPbZC6t1QbGETgyr.azZnAcJNJ/7Qdh9EasAf.
    user2:$6$zhZgcAMOe2$2CCvBRTPIsRaDA5Lt2gi3Nb6W1JQ2wklq3oZ/Cw8GNPYAtTO4CM7s/4ohNhJvzp1PpGD1WmsDf.bxBmAYTSxL0
    ```

!!! note "Note"

    Under Unix, make sure `crypt_passwd.pl` has execution rights: `chmod +x crypt_passwd.pl`
