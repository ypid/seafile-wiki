## Overview

There are generally two parts of data to backup

* Seafile library data;
* Databases

If you setup seafile server according to our manual, you should have a directory layout like:

    haiwen       # Replace the name with your organization name
      --seafile-server-2.x.x # untar from seafile package
      --seafile-data   # seafile configuration and data (if you choose the default)
      --seahub-data    # seahub data
      --ccnet          # ccnet configuration and data 
      --seahub.db      # sqlite3 database used by seahub
      --seahub_settings.py # optional config file for seahub

All your library data is stored under the 'haiwen' directory.

Seafile also stores some important metadata data in a few databases. The names and locations of these databases depends on which database software you use.

For SQLite, the database files are also under the 'haiwen' directory. The locations are:

* ccnet/PeerMgr/usermgr.db: contains user information
* ccnet/GroupMgr/groupmgr.db: contains group information
* seafile-data/seafile.db: contains library metadata
* seahub.db: contains tables used by the web front end (seahub)

For MySQL, the databases are created by the administrator, so the names can be different from one deployment to another. There are 3 databases:

* ccnet-db: contains user and group information
* seafile-db: contains library metadata
* seahub.db: contains tables used by the web front end (seahub)

The metadata in seafile db contains some pointers to the data in the 'haiwen' directory. So it's important in the backup/restore procedure to make sure the db and the data are consistent with each other.

We recommend the following backup order:

1. Dump and backup the databases;
2. Copy/rsync the data directory.

If your primary server gets broken (disk damaged) when the second step is running, you may end up with an incomplete backup on the backup server. Some pointers in the database backup point to invalid data. So we recommend you to follow two rules:

1. Backup the databases to different files each time. Do not overwrite/remove old backup dbs for at least a week. For example, you can backup the db to a file suffixed with the current date time.
2. If you have enough backup space (using tapes or de-duplication backup appliance), it's best to backup a separate copy of the data directory each time. If you use rsync for backup, don't remove or overwrite existing data on the backup server (detailed commands will be given later).

## Backing up Seafile library data and configuration ##

The data and configuration files are all stored in the `haiwen` directory, so just back up the whole directory.

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