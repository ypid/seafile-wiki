## Overview

A very simple way to backup is to stop the server and copy the whole Seafile directory to another place. If you do not stop the server, there may be some inconsistency. We provide a reliable way in the following to backup your server without the need to shut it down.

*** 

There are generally two parts of data to backup

* Seafile library data
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
2. If you use rsync for backup the data directory, don't remove or overwrite existing data on the backup server (detailed commands will be given later).

With these two rules, you can always use the older backup if the latest one is not complete.

## Backup steps ##

The backup is a three step procedure:

1. Backup the databases;
2. Backup the seafile data directory;
3. Mark the backup as complete.

We assume your seafile data directory is in `/data/haiwen` on machine A. And you want to backup to `/backup` on machine B. We'll run the backup commands from machine B to pull the data from machine A. It's recommended to create a directory layout under /backup on machine B

    /backup
    ---- databases/  contains database backup files
    ---- data/  contains backups of the data directory
    ---- markers/ contains the finish marker file for each backup

The following steps assumes you pull the backup data from machine A on machine B. You can also copy the data on machine A to machine B by mounting the backup destination via NFS or Windwos share.

### Backing up Databases ###

**MySQL**

Assume your database names are `ccnet-db`, `seafile-db` and `seahub-db`.

    B> mysqldump -h [mysqlhost] -u[username] -p[password] --opt ccnet-db > /backup/databases/ccnet-db.sql.`date +"%Y-%m-%d-%H-%M-%S"`

    B> mysqldump -h [mysqlhost] -u[username] -p[password] --opt seafile-db > /backup/databases/seafile-db.sql.`date +"%Y-%m-%d-%H-%M-%S"`

    B> mysqldump -h [mysqlhost] -u[username] -p[password] --opt seahub-db > /backup/databases/seahub-db.sql.`date +"%Y-%m-%d-%H-%M-%S"`

**SQLite**

    B> ssh admin@A "sqlite3 /data/haiwen/ccnet/GroupMgr/groupmgr.db .dump > /tmp/groupmgr.db.bak"
    B> scp admin@A:/tmp/groupmgr.db.bak /backup/databases/groupmgr.db.bak.`date +"%Y-%m-%d-%H-%M-%S"`

    B> ssh admin@A "sqlite3 /data/haiwen/ccnet/PeerMgr/usermgr.db .dump > usermgr.db.bak"
    B> scp admin@A:/tmp/usermgr.db.bak /backup/databases/usermgr.db.bak.`date +"%Y-%m-%d-%H-%M-%S"`

    B> ssh admin@A "sqlite3 /data/haiwen/seafile-data/seafile.db .dump > /tmp/seafile.db.bak"
    B> scp admin@A:/tmp/seafile.db.bak /backup/databases/seafile.db.bak.`date +"%Y-%m-%d-%H-%M-%S"`

    B> ssh admin@A "sqlite3 /data/haiwen/seahub.db .dump > /tmp/seahub.db.bak"
    B> scp admin@A:/tmp/seahub.db.bak /backup/databases/seahub.db.bak.`date +"%Y-%m-%d-%H-%M-%S"`

### Backing up Seafile library data ###

The data files are all stored in the `/data/haiwen` directory, so just back up the whole directory. You can directly copy the whole directory to the backup destination, or you can use rsync to do incremental backup. 

We use rsync on machine B to pull the directory on machine A. Supposed your data directory is `/data/haiwen` Command looks like:

    B> rsync -az user@A:/data/haiwen /backup/data

This command backup the data directory to `/backup/data/haiwen`.

### Marking the backup as complete

As mentioned before, it's important to use a complete backup for restoration. So after the above two steps finish successfully, we'll write a marker file to indicate the last backup is complete.

    B> touch /backup/markers/`date +"%Y-%m-%d-%H-%M-%S"`

The complete time of the backup is recorded in the marker file name. So you can easily tell which is the last successful backup.
 
## Restore from backup ##

Now supposed your primary seafile server is broken, you're switching to a new machine C. Using the backup data on machine B, to restore your Seafile instance:

1. Determine the latest complete backup;
2. Copy `/backup/data/haiwen` from machine B to machine C. Let's assume the seafile deployment location on machine C is also `/data/haiwen`.
3. Restore the database.

### Determine the latest complete backup

A complete backup means the database backup is consistent with the data backup.

The file names in `/backup/markers` folder records the finish time of the backup tasks. You should choose the database backup file for restoration as following:

1. Find the latest marker file in `/backup/markers`, say it's name is `2013-10-19-16-55-18`.
2. In the `/backup/databases` directory, find the database backup files with the largest time stamp before `2013-10-19-16-55-18`. Let's say they're `ccnet-db.sql.2013-10-19-16-00-05`, `seafile-db.sql.2013-10-19-16-00-20` and `seahub-db.sql.2013-10-19-16-01-05`.

### Restore the databases

Now with the latest valid database backup files at hand, you can restore them.

**MySQL**

    C> mysql -u[username] -p[password] ccnet-db < ccnet-db.sql.2013-10-19-16-00-05
    C> mysql -u[username] -p[password] seafile-db < seafile-db.sql.2013-10-19-16-00-20
    C> mysql -u[username] -p[password] seahub-db.sql.2013-10-19-16-01-05

**SQLite**

    C> cd /data/haiwen
    C> mv ccnet/PeerMgr/usermgr.db ccnet/PeerMgr/usermgr.db.old
    C> mv ccnet/GroupMgr/groupmgr.db ccnet/GroupMgr/groupmgr.db.old
    C> mv seafile-data/seafile.db seafile-data/seafile.db.old
    C> mv seahub.db seahub.db.old
    C> sqlite3 ccnet/PeerMgr/usermgr.db < usermgr.db.bak.xxxx
    C> sqlite3 ccnet/GroupMgr/groupmgr.db < groupmgr.db.bak.xxxx
    C> sqlite3 seafile-data/seafile.db < seafile.db.bak.xxxx
    C> sqlite3 seahub.db < seahub.db.bak.xxxx