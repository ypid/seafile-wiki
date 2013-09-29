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
    rsync -azv user@A:/path/to/haiwen /backup/

Also you might want to set up a script framework that calls such a command via `cron`. Rule looks like:

     0 3 * * * rsync -azv user@A:/path/to/haiwen /backup/

This will perform backup operation on 3:00 am everyday.     

### Backing up Database ###

Small amount (but important) metadata of the libraries is stored in database. It's critical that the metadata in database is consistent with the data in the seafile-data folder. So it's recommended to keep the backups for a past period (e.g. a week). In case the latest copy of backup is inconsistent with the data, you can still use an older one.

**MySQL**

    mysqldump -u[username] -p[password] --opt ccnet-db > ccnet-db.sql.2013-10-01

    mysqldump -u[username] -p[password] --opt seafile-db > seafile-db.sql.2013-10-01

    mysqldump -u[username] -p[password] --opt seahub-db > seahub-db.sql.2013-10-01

**SQLite**

    sqlite3 ccnet/GroupMgr/groupmgr.db .dump > groupmgr.db.bak.2013-10-01

    sqlite3 ccnet/PeerMgr/usermgr.db .dump > usermgr.db.bak.2013-10-01

    sqlite3 seafile-data/seafile.db .dump > seafile.db.bak.2013-10-01
    
    sqlite3 seahub.db .dump > seahub.db.bak.2013-10-01

## Recovering from backup ##

Recovery is what really matters. To restore your Seafile instance:

1. Copy over your `haiwen` folder with your backed up `haiwen` folder

2. Import your databases