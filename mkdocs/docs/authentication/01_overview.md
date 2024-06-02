# Overview
MRBS features various authentication methods, which are explained here.

## Principles
### User interface

MRBS pages can be accessed and used, depending on the user's access level.

There are three levels of access in MRBS:

| Level | Description        | Permissions                                                  |
| ----- | ------------------ | ------------------------------------------------------------ |
| 0     | Unkown user        | Can view most pages, but cannot make any change.             |
| 1     | Authenticated user | Can make bookings, and can change their own bookings.        |
| 2     | Administrator      | Are allowed to modify other people's bookings. Also have the ability to add and remove rooms and areas. |

**Authentication process**

1. Before accessing a restricted page, MRBS prompts the user for a name and password. 
2. Then it connects to an authentication server, which is responsible for deciding, whether the user/password pair is valid. 
3. If it is, MRBS then checks if the name is in its list of administrators. 
4. Finally, MRBS grants access to the page at the right level. The name and password are recorded for the duration of the session, and won't be asked for again.

### Configuration

MRBS authentication configuration is done in file `config.inc.php`. This file contains a section for authentication parameters, where several
choices must be made:

- The session management scheme. (How to prompt for a user/password, and how keep track of it)
- The authentication method. (What kind of server will verify the user/password validity)
- The list of administrators (not used by the 'db' authentication method)

!!! tip "Alternative authentication methods"
    There are several alternative authentication methods. They differ by the type of authentication server they can connect to, and by the kind of program used for communicating with that server. Each method is implemented in a pluggable module. There are two general categories:

    - Internal PHP modules, which connect directly to an authentication server. These modules may require additional parameter settings in `config.inc.php`.

        **Supported kinds of authentication servers:**

        - LDAP servers
        - POP3 email servers
        - IMAP email servers
        - NW servers
        - In addition, three special internal modules support a local list of users for one; a local list of users stored in the database for the second; and authenticating anybody for the other.
    
    - External programs, that do the same after being invoked by the special `ext` PHP authentication module.

        **Supported**

        - NT Domain controllers
        - Netware servers
        - PAM servers

## Which authentication scheme to choose
The default authentication scheme is `db`, which stores users' details in the MRBS database in the `users` table.  However, the right choice for you will depend on whether your users are already listed somehere else.

If they're all in a closed organization (eg a private company or a university), then this organization probably already has its own authentication service. MRBS can connect to several kinds of services, such as LDAP servers, NT domain controllers, Netware servers and POP3 or IMAP mail servers.

If your website is using a Joomla! or WordPress CMS and you already require your users to authenticate, then MRBS can be configured to authenticate your users against the existing list of users in the CMS.

### PHP authentication modules
| Type     | Description                                                                                                                 | Pros                                                                                                                                       | Cons                                                                                                                                                             |
| -------- | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| None     | Every user is accepted. This was for example the case of the MRBS 1.1 IP address and computer name "authentications".       | Very simple to setup.                                                                                                                      | No security at all.                                                                                                                                              |
| config   | Users are listed in `config.inc.php`.                                                                                       | <ul><li>Simple to setup.</li><li>Not dependent on external authentication server, so usable on the Internet.</li></ul>                     | <ul><li>Users cannot update their own password.</li><li>Administrators manually set the user passwords, which is against normal confidentiality rules.</li></ul> |
| db       | Users validated using web-based authentication based on a table in MRBS database.                                           | <ul><li>Simple to setup.</li><li>Built in MRBS but more secure than 'config'.</li><li>Easy to configure.</li></ul>                         | Does not use existing directory.                                                                                                                                 |
| db_ext   | Users validated using a table in an external database.                                                                      | <ul><li>Uses an existing authentication database thereby saving directory duplication.</li><li>Easy to configure.</li></ul>                |                                                                                                                                                                  |
| ldap     | Users validated using LDAP directory services.                                                                              | For corporate intranets using LDAP directory services.                                                                                     | Does not work on the Internet.                                                                                                                                   |
| pop3     | Users validated by a POP3 mail server.                                                                                      | For groups of users all having an Email address on the same server.                                                                        | Technically works on the Internet, but it's unlikely all users of a site will have an Email address on the same mail server.                                     |
| imap     | Users validated by an IMAP mail server.                                                                                     | For groups of users all having an Email address on the same server.                                                                        | Technically works on the Internet, but it's unlikely all users of a site will have an Email address on the same mail server.                                     |
| imap_php | A more up-to-date version of IMAP.                                                                                          |                                                                                                                                            |                                                                                                                                                                  |
| nw       | Users validated by Netware server (user contrib.).                                                                          | ?                                                                                                                                          | This is only going to work on Linux.                                                                                                                             |
| ext      | Validation is delegated to an external program.                                                                             | Lots of possibilities.                                                                                                                     | Most available programs work only under Unix.                                                                                                                    |
| joomla   | Users are validated against a Joomla! installation running on the same server.                                              |                                                                                                                                            |                                                                                                                                                                  |
| wix      | Users are validated against members registered on a Wix website.                                                            | <ul><li>Users don't need to have two sets of login details.</li><li>Administrators only have to manage users in one place (Wix).</li></ul> | <ul><li>Not a single sign-on/sign-off solution.</li><li>Can be slight delays in retrieving user details from a remote Wix site.</li></ul>                        |
| wordpress| Users are validated against a WordPress installation running on the same server.                                            |                                                                                                                                            |                                                                                                                                                                  |
| cas      | Users are validated against a CAS server.                                                                                   | <ul><li>Federated login.</li><li>Single sign-on.</li><li>Single sign-off.</li></ul>                                                        | Requires a CAS server.                                                                                                                                           |
| saml     | Users are validated against a SimpleSamlPhp installation running on the same server.                                        | <ul><li>Federated login.</li><li>Single sign-on.</li><li>Single sign-off.</li><li>Hooks into SAML infrastructure.</li></ul>                | <ul><li>Must set up SimpleSamlPhp.</li><li>SAML knowledge recommended.</li></ul>                                                                                 |
| idcheck  | For use with [`mod_idcheck`](https://idcheck.sourceforge.net). Must be used together with the `remote_user` session scheme. |                                                                                                                                            |                                                                                                                                                                  |

### External authentication programs
| Program          | Description                                                                         | Pros                                                                                | Cons                              |
| ---------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | --------------------------------- |
| crypt_passwd.pl  | Perl script which reads a `shadow` style file with usernames and crypted passwords. | Easy to setup                                                                       | Relies upon a special users file. |
| auth_pam.pl      | Uses PAM                                                                            | For Unix system uses PAM (especially Linux, but also SUN Solaris)                   |                                   |

## Which session scheme to choose
The session scheme is the way the user and password is queried and recorded.

The default session scheme is `php`.  Some authentication schemes, e.g. 'joomla', 'wordpress', 'cas' and 'saml' require you to use their own session schemes. The others are left in for historical reasons:

| Type        | Description                                                                                                                                                                                                                                                                                            | Pros                                                                                                             | Cons                                                                                                                                               |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| php         | Use PHP's native session handling. Recommended. Session data is saved in the MRBS database.                                                                                                                                                                                                            | Recommended by PHP doctors, PHP moms, etc.                                                                       | Any?                                                                                                                                               |
| http        | Use the "HTTP basic authentication" protocol to get a user/password popup.                                                                                                                                                                                                                             | <ul><li>Simple.</li><li>This was the default in MRBS 1.1 for most authentication schemes.</li></ul>              | <ul><li>Incompatible with IIS web servers.</li><li>No way to log out.</li></ul>                                                                    |
| cookie      | Save the user/password in cookies on the client's web browser.                                                                                                                                                                                                                                         | Less demanding for the server than PHP native sessions. (No files stored)                                        | Some users disable cookies on their browser.                                                                                                       |
| nt          | The user's identity is queried AND validated by an NT/IIS server running in authenticated access mode. (That is anonymous access disabled, or Access Control Lists enabled) Use in combination with authentication "none", as the authentication is already done by IIS during the session initiation. | For corporate intranets using NT/2000/XP servers in authenticated access mode.                                   | <ul><li>Incompatible with Linux servers by definition.</li><li>Does not work on the Internet.</li><li>Does not allow anonymous browsing.</li></ul> |
| omni        | The user's identity is queried AND validated by an Omnicron OmniHTTPd web server. Use in combination with authentication "none", as authentication is already done by OmniHTTPd during the session initiation.                                                                                         | For users of Omnicron OmniHTTPd web servers.                                                                     | <ul><li>For users of Omnicron OmniHTTPd web servers.</li></ul>                                                                                     |
| remote_user | The user's identity is determined by reading the REMOTE_USER environment variable. Use in combination with authentication "none", as authentication has already been done by the system that sets REMOTE_USER.                                                                                         | For users that already have a wider auth. scheme that sets REMOTE_USER, allows MRBS to use that scheme.          | <ul><li>Requires a web server setup that sets REMOTE_USER. Could be hard to set up.</li></ul>                                                      |
| ip          | Users are identified by the IP address of their computer. Use in combination with authentication "none" or "config".                                                                                                                                                                                   | Easy to setup, for MRBS evaluation.                                                                              | <ul><li>Incompatible with DHCP.</li><li>Users cannot make changes from a different computer.</li></ul>                                             |
| host        | Users are identified by the DNS name of their host computer. Use in combination of authentication "none" or "config".                                                                                                                                                                                  | Easy to setup, for MRBS evaluation.                                                                              | Users cannot make changes from a different computer.                                                                                               |
| joomla      | Use when using Joomla! authentication.                                                                                                                                                                                                                                                                 |                                                                                                                  |                                                                                                                                                    |
| wordpress   | Use when using WordPress authentication.                                                                                                                                                                                                                                                               |                                                                                                                  |                                                                                                                                                    |
| cas         | Use when using CAS authentication.                                                                                                                                                                                                                                                                     | <ul><li>Federated login.</li><li>Single signon.</li><li>Single signoff.</li></ul>                                | Requires a CAS server.                                                                                                                             |
| saml        | Use when using SAML authentication.                                                                                                                                                                                                                                                                    | <ul><li>Federated login.</li><li>Single signon.</li><li>Single signoff.</li><li>Hooks into SAML infra.</li></ul> | <ul><li>Must set up SimpleSamlPhp.</li><li>SAML knowledge recommended.</li></ul>                                                                   |

## A little bit of history
??? quote "History"

    The original version of MRBS, created by Daniel Gardner, did not have username/password support. Each booking that was made had the IP address of the   client machine logged as the "creator" of the booking.

    In this case the IP address functions as the "username".

    This works well in the case where all IPs are static, and everyone is allowed access to the system. In the wider world there are occasions where this is a  little too liberal.

    To combat this MRBS 0.8 introduced authentication, initially provided by [Sam Mason](smason@mtc.ricardo.com).

    There are three levels of access in MRBS:

    - Level 0 - not logged in
    - Level 1 - Authenticated
    - Level 2 - Administrator

    The administrators are allowed to modify other people's bookings, whereas Level 1 users can only change their own. Administrators also have the ability to  add and remove rooms and areas.

    With time, several alternative authentication methods were introduced. All rely on the same basic principles:

    - Each user is identified by a name.
    - Each name has a corresponding password.
    - The name/password pair must be valid in the authentication service MRBS connects to.
    - Some users have additional MRBS administration rights. Their names
    are listed in the $auth["admin"] list in config.inc.php.
    - It's the web site creator's responsibility to manually enter the names of all the users that will have administrator rights in this list.

    Before accessing a restricted page, MRBS prompts the user for a name and
    password. Then it connects to an authentication server, which is responsible
    for deciding if the user/password pair is valid. If it is, MRBS finally grants
    access to the page.

    The types of authentication servers supported are: LDAP servers; NT domain
    controllers; Netware servers; POP3 email servers; IMAP email servers; etc.

    Still the initial authentication system had shortcomings:

    - The user/password prompt relied on the HTTP basic authentication protocol, in a mode not supported by IIS web servers.
    - MRBS was increasingly used on the Internet, where central authentication servers are not available in the general case.
    - There was no way to log off.

    In MRBS 1.2, the authentication system was restructured by [Jean-François Larvoire](jf.larvoire@sf.net)

    The main change was to separate the user/password acquisition (Also called session initiation), from its validation (called authentication).

    The existing routines for querying the user identity were moved to module `session_http.inc` (now `SessionHttp.php`). This module is left in for backwards compatibility, but deprecated. A new session management module was introduced, based on PHP's built-in session management. This is the one recommended now on. It includes the ability to log off.

    On the authentication side, most existing modules were carried on, with the session code removed. A simple module, auth_config.inc (now AuthConfig.php) was added, managing a simple list of users in config.inc.php. Eventually there will also be another authentication module, using a table of users in the MRBS database.
