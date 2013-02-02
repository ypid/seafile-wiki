**Note:** Do not forget to update your Nginx/Apache config file if you deploy seafile with them.

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

## Minor upgrade (like from 1.3.0 to 1.3.1)

Minor upgrade is like an upgrade from 1.3.0 to 1.3.1. For this type of upgrade, you only need to update the avatar link:

<pre>
cd haiwen/seafile-server-1.3.1/seahub/media
cp -rf avatars/* ../../../seahub-data/avatars/
rm -rf avatars
ln -s ../../../seahub-data/avatars
</pre>
