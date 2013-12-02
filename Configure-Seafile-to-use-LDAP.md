The current code of seahub assumes that user name to be email address, so it's not possible to log in with UNIX user names or Windows Domain user names now. The support may be added later.

Note that the seafile admin user account is always stored in sqlite/mysql database, not in LDAP.

If you have used Seafile without configuring LDAP before, the users are stored in database (MySQL or SQLite). Starting from 2.0.4, after enabling LDAP, the users in database are still valid. Seafile will find the user both from database and LDAP. Seafile server before 2.0.4 can only find users from either database or LDAP, not both.

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

## Multiple base DN/Additional search filter

Multiple base DN is useful when your company has more than one OUs to use Seafile. You can specify a list of base DN in the "BASE" config. The DNs are separated by ";", e.g. `cn=developers,dc=example,dc=com;cn=marketing,dc=example,dc=com`

Search filter is very useful when you have a large organization but only a portion of people want to use Seafile. The filter can be given by setting "FILTER" config. For example, add the following line to LDAP config:

```
FILTER = memberOf=CN=group,CN=developers,DC=example,DC=com
```

Note that the cases in the above example is significant.

## Known Issues

### ldaps (LDAP over SSL) doesn't work on Ubuntu/Debian

Please see https://github.com/haiwen/seafile/issues/274

This is because the pre-compiled package is built on CentOS. The libldap we distribute is from CentOS so it search for config files on /etc/openldap/ldap.conf. But Ubuntu/Debian uses /etc/ldap/ldap.conf. So the path to the CA files can't be found on the client.

The work around is 

```
mkdir /etc/openldap && ln -s /etc/ldap/ldap.conf /etc/openldap/
```