## Continuous Upgrade

Continuous upgrade means to upgrade from one version of Seafile server to the next version.
For example, upgrading from 1.2.0 to 1.3.0 is a continuous upgrade.

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
    $ ./upgrade_1.2.0_1.3.0_server.sh

The naming rule for upgrade scripts is upgrade_a.b.c_x.y.z_server.sh,
where a.b.c is the old version, and x.y.z is the new version.

## Noncontinuous Upgrade

You may also upgrade a few versions at once, e.g. from 1.1.0 to 1.3.0.
The procedure is:

1. upgrade from 1.1.0 to 1.2.0;
2. upgrade from 1.2.0 to 1.3.0.

Just run the upgrade scripts in sequence. (You don't need to download server package 1.2.0)