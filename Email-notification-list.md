Seafile will send email to a user in the following cases:

| Category | When | Subject | Body |
|----------|------|------| -----|
| User Management | User reset his/her password | seahub/seahub/auth/forms.py line:103 | seahub/seahub/templates/registration/password_reset_email.html |
|                 | System admin add new member      | seahub/seahub/views/sysadmin.py line:424| seahub/seahub/templates/sysadmin/user_add_email.html|
|                 | System admin reset user password | seahub/seahub/views/sysadmin.py line:368 | seahub/seahub/templates/sysadmin/user_reset_email.html |
| File Sharing    | User send file/folder share link | seahub/seahub/share/views.py line:668 | seahub/seahub/templates/shared_link_email.html|

**Note:** Subject line may vary between different releases, this is based on Release 2.0.1.