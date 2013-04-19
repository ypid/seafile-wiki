Starting from seafile server **1.5** (please upgrade if your're not running 1.5+), Seafile supports authenticating users from LDAP.

The current code of seahub assumes that user name to be email address, so it's not possible to log in with UNIX user names or Windows Domain user names now. The support may be added later.

Note that the seafile admin user account is always stored in sqlite/mysql database, not in LDAP.

To use LDAP to authenticate user, please add the following lines to ccnet.conf

    [LDAP]
    # LDAP URL for the host, ldap://, ldaps:// and ldapi:// are supported
    # To use TLS, you should configure the LDAP server to listen on LDAPS
    # port and specify ldaps:// here.
    HOST = ldap://ldap.example.com
    # base DN from where all the users can be reached.
    BASE = ou=users,dc=example,dc=com
    # user DN to bind to. If not set, will use anonymous login.
    USER_DN = cn=seafileadmin,dc=example,dc=com
    # Password for the user DN
    PASSWORD = secret
    # The attribute to be used as user login id.
    # By default it's the 'mail' attribute.
    # Note that only email can be used as user name for now.
    LOGIN_ATTR = mail

Example config for LDAP:

    [LDAP]
    HOST = ldap://192.168.1.123/
    BASE = ou=users,dc=example,dc=com
    USER_DN = cn=admin,dc=example,dc=com
    PASSWORD = secret
    LOGIN_ATTR = mail

Example config for Active Directory:

    [LDAP]
    HOST = ldap://192.168.1.123/
    BASE = cn=users,dc=example,dc=com
    USER_DN = cn=admin,cn=users,dc=example,dc=com # or use admin@example.local etc
    PASSWORD = secret
    LOGIN_ATTR = mail