## Backing up Seafile data ##

If you setup seafile server according to our manual, you should have a directory layout like:

    haiwen       # Replace the name with your organization name
      --seafile-server-1.2.0 # untar from seafile package
      --seafile-data   # seafile configuration and data (if you choose the default)
      --seahub-data    # seahub data
      --ccnet          # ccnet configuration and data 
      --seahub.db      # sqlite3 database used by seahub
      --seahub_settings.py # optional config file for seahub

Seafile data are all stored in the `haiwen` directory, so just back up the whole directory.

We use rsync on our backup machine to pull the directory on machine A. Command looks like:

    mkdir /backup
    rsync -azv --delete user@A:/path/to/haiwen /backup/

Also you might want to set up a script framework that calls such a command via `cron`. Rule looks like:

     0 3 * * * rsync -azv --delete user@A:/path/to/haiwen /backup/

This will perform backup operation on 3:00 am everyday.     

### Backing up Database ###

**MySQL**

    mysqldump -u[username] -p[password] --opt ccnet-db > ccnet-db.sql

    mysqldump -u[username] -p[password] --opt seafile-db > seafile-db.sql

    mysqldump -u[username] -p[password] --opt seahub-db > seahub-db.sql

**SQLite**

    sqlite3 ccnet/GroupMgr/groupmgr.db .dump > groupmgr.db.bak

    sqlite3 ccnet/PeerMgr/usermgr.db .dump > usermgr.db.bak

    sqlite3 seafile-data/seafile.db .dump > seafile.db.bak
    
    sqlite3 seahub.db .dump > seahub.db.bak

## Recovering from backup ##

Recovery is what really matters. To restore your Seafile instance:

1. Copy over your `haiwen` folder with your backed up `haiwen` folder

2. Import your databases
