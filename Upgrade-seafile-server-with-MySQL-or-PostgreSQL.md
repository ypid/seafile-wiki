
## Major Continuous Upgrade (like from 1.2 to 1.3)

Continuous upgrade means to upgrade from one version of Seafile server to the next version.
For example, upgrading from 1.2.0 to 1.3.0 (or upgrading from 1.2.0 to 1.3.1) is a continuous upgrade.

**Note:** Minor upgrade, like upgrade from 1.3.0 to 1.3.1, is documented in a separate section below.

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

### Update Nginx/Apache Config

For Nginx:

```
  location /media {
      root /data/haiwen/seafile-server-1.6.0/seahub;
  }
```

For Apache:

```
Alias /media  /data/haiwen/seafile-server-1.6.0/seahub/media
```

### Restart Seafile/Seahub/Nginx/Apache

After done above updating, now restart Seafile/Seahub/Nginx/Apache to see the new version at work!

## Noncontinuous Upgrade (like from 1.1 to 1.3)

You may also upgrade a few versions at once, e.g. from 1.1.0 to 1.3.0.
The procedure is:

1. upgrade from 1.1.0 to 1.2.0;
2. upgrade from 1.2.0 to 1.3.0.


## Minor upgrade (like from 1.5.0 to 1.5.1)

Minor upgrade is like an upgrade from 1.5.0 to 1.5.1. 

Here is our dir strutcutre

<pre>
haiwen
   -- seafile-server-1.5.0
   -- seafile-server-1.5.1
   -- ccnet
   -- seafile-data
</pre>

1. Stop the current server first as for any upgrade 
```sh
seafile-server-1.5.0/seahub.sh stop
seafile-server-1.5.0/seafile.sh stop
```

1. For this type of upgrade, you only need to update the avatar link. We provide a script for you, just run it:
```sh
cd seafile-server-1.5.1
upgrade/minor-upgrade.sh
```

1. Start the new server version as for any upgrade 
```sh
cd ..
seafile-server-1.5.1/seafile.sh start
seafile-server-1.5.1/seahub.sh start
```

1. Update Nginx/Apache Config

For Nginx:

```
  location /media {
      root /data/haiwen/seafile-server-1.6.0/seahub;
  }
```

For Apache:

```
Alias /media  /data/haiwen/seafile-server-1.6.0/seahub/media
```
