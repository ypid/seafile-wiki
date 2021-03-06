# Deploy Seafile with MySQL #

If you need to deploy Seafile with MySQL, you need to download **seafile-server-1.4.5 or above**.

**Note:** make sure that seafile is started by `./seafile.sh start` and LDAP is not enabled, otherwise seahub admin will not be created correctly.

## Preparation ##

1. [[Download and setup seafile server]], then start seafile and seahub and make sure everything is OK.

2. Setup MySQL, we assume MySQL user is `root` and password is `root` in our example below.

3. Create 3 databases named `ccnet-db`, `seafile-db`, `seahub-db`. e.g., ``create database `ccnet-db` character set = 'utf8';``

## Steps ##

1. Shutdown services by `./seahub.sh stop` and `./seafile.sh stop`. Then, append MySQL the following configurations to 3 config files (you may need to change to fit your configuration).

    Append following lines to `ccnet/ccnet.conf`:

        [Database]
        ENGINE=mysql
        HOST=127.0.0.1
        USER=root
        PASSWD=root
        DB=ccnet-db

    Note: Use `127.0.0.1`, don't use `localhost`.    

    Replace the database section in `seafile-data/seafile.conf` with following lines:

        [database]
        type=mysql
        host=127.0.0.1
        user=root
        password=root
        db_name=seafile-db

    Append following lines to `seahub_settings.py`:

        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'USER' : 'root',
                'PASSWORD' : 'root',
                'NAME' : 'seahub-db',
                'HOST' : '127.0.0.1', 
                'OPTIONS': {
                    "init_command": "SET storage_engine=INNODB",
                }
            }
        }

2. Start seafile by `./seafile.sh start`. There will be several tables created in `ccnet-db` and `seafile-db` if your configuration is correct.

3. Install python-mysqldb (package name on ubuntu):

        sudo apt-get install python-mysqldb

4. Start seahub as follows (assume current path is `/data/haiwen/seafile-server-1.4`:

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

This command tool will guide you to create a seahub admin.

**Note:** this command is added since version 1.4.5.

## Upgrading ##

### Update avatars symbolic link ###

Assume your top level directory is `/data/haiwen/`, and you are upgrading to seafile server version 1.6.0:

```
cd /data/haiwen
cp -a seafile-server-1.6.0/seahub/media/avatars/* seahub-data/avatars/
rm -rf seafile-server-1.6.0/seahub/media/avatars
#the new server avatars' folder will be linked to the updated avatars folder
ln -s -t seafile-server-1.6.0/seahub/media/  ../../../seahub-data/avatars/
```

### Update database tables  ###

When a new version of seafile server is released, there may be changes to the database of seafile/seahub/ccnet. We provide the sql statements to update the databases:

- `upgrade/sql/<VERSION>/mysql/seahub.sql`, for changes to seahub database
- `upgrade/sql/<VERSION>/mysql/seafile.sql`, for changes to seafile database
- `upgrade/sql/<VERSION>/mysql/ccnet.sql`, for changes to ccnet database

To apply the changes, just execute the sqls in the correspondent database. If any of the sql files above do not exist, it means the new version does not bring changes to the correspondent database.

```sh
seafile-server-1.6.0
├── seafile
├── seahub
├── upgrade
    ├── sql
        ├── 1.6.0
            ├── mysql
                ├── seahub.mysql
                ├── seafile.mysql
                ├── ccnet.mysql
```