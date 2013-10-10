Steps to migrate Seafile from SQLite to MySQL:

1. Change your configuration files according https://github.com/haiwen/seafile/wiki/Deploy-Seafile-with-MySQL

2. Download scripts from https://github.com/haiwen/seafile/blob/master/scripts/sqlite2mysql.sh and https://github.com/haiwen/seafile/blob/master/scripts/sqlite2mysql.py

3. Run `sqlite2mysql.sh` by

    ./sqlite2mysql.sh
    
  this script will ask your Seafile install path, then will produce 3 files: `ccnet-db.sql`, `seafile-db.sql`, `seahub-db.sql`

4. Loads the sql files to your MySQL databases which should be created in advance. For example:

    mysql> source ccnet-db.sql

5. Restart seafile and seahub

    

