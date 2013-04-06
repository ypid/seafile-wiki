**Note:** Do not forget to [update your Nginx/Apache config file](https://github.com/haiwen/seafile/wiki/Deploy-Seafile-Web-with-nginx-apache#notes-when-upgrading-seafile-server) if you deploy seafile with them.

This page is for users who use the pre-compiled seafile server package. If you [build seafile server from source](Build and Deploy Seafile Server from source), please read the **Upgrading Seafile Server** section on that page, instead of this one.

## Continuous Upgrade (like from 1.2 to 1.3)

Continuous upgrade means to upgrade from one version of Seafile server to the next version.
For example, upgrading from 1.2.0 to 1.3.0 is a continuous upgrade.

**Note:** Minor upgrade, like upgrade from 1.3.0 to 1.3.1, is documented in a separate section below.

Let's use an example to go through the process. Supposed you installed Seafile server 1.2.0
following the steps in [[Download and Setup Seafile Server]], you should have a directory
layout similar to this:

<pre>
haiwen
   -- seafile-server-1.2.0
   -- ccnet
   -- seafile-data
</pre>

Now you want to upgrade to version 1.3.0.

First, you should shutdown Seafile server if it's running

    $ cd haiwen/seafile-server-1.2.0
    $ ./seahub.sh stop
    $ ./seafile.sh stop

Then download seafile-server_1.3.0_x86-64.tar.gz and extract it to `haiwen` directory.
Now you have the following directory layout:

<pre>
haiwen
   -- seafile-server-1.2.0
   -- seafile-server-1.3.0
   -- ccnet
   -- seafile-data
</pre>

Finally run the upgrade script in seafile-server-1.3.0 directory.

    $ cd haiwen/seafile-server-1.3.0/upgrade
    $ ./upgrade_1.2_1.3_server.sh

The naming rule for upgrade scripts is `upgrade_a.b_x.y.sh`,
where `a.b` is the old version, and `x.y` is the new version.

## Noncontinuous Upgrade (like from 1.1 to 1.3)

You may also upgrade a few versions at once, e.g. from 1.1.0 to 1.3.0.
The procedure is:

1. upgrade from 1.1.0 to 1.2.0;
2. upgrade from 1.2.0 to 1.3.0.

Just run the upgrade scripts in sequence. (You don't need to download server package 1.2.0)

## Minor upgrade (like from 1.5.0 to 1.5.1)
Minor upgrade is like an upgrade from 1.5.0 to 1.5.1. 

1. Here is our dir strutcutre
```sh
cd haiwen
tree -L 1
.
├── app
├── ccnet
├── seafile-data
├── seafile-server-1.5.0
├── seafile-server-1.5.1
├── seahub-data
├── seahub.db
├── seahub_settings.py
└── seahub_settings.pyc
```
1. Stop the current server first as for any upgrade 
```sh
seafile-server-1.5.0/seahub.sh stop
seafile-server-1.5.0/seafile.sh stop
```
1. For this type of upgrade, you only need to update the avatar link.
```sh
rm -rf seafile-server-1.5.1/seahub/media/avatars
#the new server avatars' folder will be linked to the updated avatars folder
ln -s -t seafile-server-1.5.1/seahub/media/  ../../../seahub-data/avatars/  
```

1. Start the new server version as for any upgrade 
```sh
seafile-server-1.5.1/seafile.sh start
seafile-server-1.5.1/seahub.sh start
```