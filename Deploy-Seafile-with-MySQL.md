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

## Issues with the upgrades ##

We will add a file named `1.x_1.y.mysql` which contains all the upgrade statements in our release.

After updating database, you also need to manually update avatar symbol link.

```
cd haiwen/seafile-server-1.4/seahub/media
cp -rf avatars/* ../../../seahub-data/avatars/
rm -rf avatars
ln -s ../../../seahub-data/avatars
```