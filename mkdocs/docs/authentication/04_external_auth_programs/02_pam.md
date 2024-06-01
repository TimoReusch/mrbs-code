# PAM

In case your Unix system uses PAM (especially Linux, but also SUN Solaris) you might try the script `auth_pam.pl`. This script takes two two parameters - the user name and the user password.

??? abstract "auth_pam.pl"

    ```bash
    #!/usr/bin/sperl5.6.0

    # auth_pam.pl
    # uses Authen::PAM to validate a user - password pair
    # usage: auth_pam.pl [user] [password]
    # exit 0 on success, otherwise 1
    # script has to be SUID and use sperl if run as an unprivileged use
    # handle with care ...
    # Michael Redinger

    use Authen::PAM;

    exit 1 unless ( $ARGV[0] && $ARGV[1] );
    my $service = "passwd";
    my $username = $ARGV[0];
    my $password = $ARGV[1];

    sub my_conv_func {
    	my @res;
    	while ( @_ ) {
    		my $code = shift;
    		my $msg = shift;
    		my $ans = "";

    		$ans = $username if ($code == PAM_PROMPT_ECHO_ON() );
    		$ans = $password if ($code == PAM_PROMPT_ECHO_OFF() );

    		push @res, (PAM_SUCCESS(),$ans);
    	}
    	push @res, PAM_SUCCESS();
    	return @res;
    }

    ref(my $pamh = new Authen::PAM($service, $username, \&my_conv_func)) ||
    	die "Error code $pamh during PAM init!";

    my $ret=$pamh->pam_authenticate;

    exit 1 if ( $ret != 0 );
    ```

This perl script requires `Authen::PAM`.

The script has to be run as `SUID`. Therefore you should take some precautions to not undermine your system's security. Change the group of the script to that of your Web Server (for Apache this is normally nobody or apache) and don't make it readable and executable to anybody else, eg.:

```bash
chgrp nobody auth_pam.pl
```

The script has to be run SUID and has to be executed by sperl:

```bash
chmod 4750 auth_pam.pl
```

The first line (`#!/usr/bin/sperl5.6.0`) says where sperl is located on your system. Please adapt this for your needs.

Now add the configuration to `web/config.inc.php` in your MRBS directory. This looks as follows (adapt the path to auth_pam.pl according to your your configuration):

```php-inline
$auth["realm"] = "MY REALM";

$auth["type"] = "ext";
$auth["prog"] = "/var/www/html/mrbs/auth_pam.pl";
$auth["params"] = "#USERNAME# #PASSWORD#";
```

That's it.

!!! warning "Warning"

    This has been only tested with Red Hat 7.x. Feedback on whether this works on other systems (eg. Solaris) is appreciated.
