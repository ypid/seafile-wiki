# Deploy Seafile with MySQL #

If you need to deploy Seafile with MySQL, you need to download **seafile-server-1.4.5 or above**.

## Preparation ##

1. [[Download and setup seafile server]], then start seafile and seahub and make sure everything is OK.

2. Setup MySQL, we assume MySQL user is `root` and password is `root` in our example below.

3. Create 3 databases named `ccnet-db`, `seafile-db`, `seahub-db`. e.g., ``create database `ccnet-db` character set = 'utf8';``

## Steps ##

1. Shutdown services by `./seahub.sh stop` and `./seafile.sh stop`. Then, append MySQL configurations to 3 config files (you may need to change to fit your configuration).

    Append following lines to `ccnet/ccnet.conf`:

        [Database]
        ENGINE=mysql
        HOST=localhost
        USER=root
        PASSWD=root
        DB=ccnet-db
        UNIX_SOCKET=/var/lib/mysql/mysql.sock
    
    Replace the database section in `seafile-data/seafile.conf` with following lines:

        [database]
        type=mysql
        host=localhost
        user=root
        password=root
        db_name=seafile-db
        unix_socket=/var/lib/mysql/mysql.sock

    Append following lines to `seahub_settings.py`:

        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'USER' : 'root',
                'PASSWORD' : 'root',
                'NAME' : 'seahub-db',
                'HOST' : '/var/lib/mysql/mysql.sock',
                'OPTIONS': {
                    "init_command": "SET storage_engine=INNODB",
                }
            }
        }

2. Start seafile by `./seafile.sh start`. There will be several tables created in `ccnet-db` and `seafile-db` if your configuration is correct.

3. Start seahub as follows (assume current path is `/data/haiwen/seafile-server-1.4`, and python-mysqldb (package name on ubuntu) installed):

        export CCNET_CONF_DIR=/data/haiwen/ccnet
        export SEAFILE_CONF_DIR=/data/haiwen/seafile-data
        INSTALLPATH=/data/haiwen/seafile-server-1.4
        export PYTHONPATH=${INSTALLPATH}/seafile/lib/python2.6/site-packages:${INSTALLPATH}/seafile/lib64/python2.6/site-packages:${INSTALLPATH}/seahub/thirdpart:$PYTHONPATH
        cd seahub
        python manage.py syncdb
    
    There will be several tables created in `seahub-db`. Then start seahub by `./seahub.sh start`.

## Create Seahub Admin ##

Assume current path is `/data/haiwen/seafile-server-1.4`, and you have exported all the variables above, 

    cd seahub
    python manage.py createsuperuser

This command tool will guild you to create a seahub admin.

**Note:** this command is added since version 1.4.5.

## How to Upgrades ##

We will add a file named `1.x_1.y.mysql` which contains all the upgrade statements in our release.

After updating database, you also need to manually update avatar symbol link.

```
cd haiwen/seafile-server-1.4/seahub/media
#the new server avatars' folder will be linked to the updated avatars folder
ln -s -t seafile-server-1.5.1/seahub/media/  ../../../seahub-data/avatars/  
```


## Upgrading ##

### Update avatars symbol link ###

Assume your top level directory is `/data/haiwen/`, and you are upgrading to seafile server version 1.6.0:

```
cd /data/haiwen
cp -a seafile-server-1.6.0/seahub/media/avatars/* seahub-data/avatars/
rm -rf seafile-server-1.6.0/seahub/media/avatars
#the new server avatars' folder will be linked to the updated avatars folder
ln -s -t seafile-server-1.6.0/seahub/media/  ../../../seahub-data/avatars/
```

### Update database tables  ###

When a new version of seafile server is released, there may be changes to the database of seafile/seahub. 
We provide the sql statements to update the databases:

- `upgrade/mysql/<VERSION>/seahub.sql`, for changes to seahub database
- `upgrade/mysql/<VERSION>/seafile.sql`, for changes to seafile database
- `upgrade/mysql/<VERSION>/ccnet.sql`, for changes to ccnet database

To apply the changes, just execute the sqls in the correspondent database. If any of the sql files above does not exist, it means the new version does not bring changes to the correspondent database.

```sh
seafile-server-1.6.0
├── seafile
├── seahub
├── upgrade
    ├── mysql
        ├── 1.6.0
            ├── seahub.mysql
            ├── seafile.mysql
            ├── ccnet.mysql
```