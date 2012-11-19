## What seafile-admin command does ##

* as is explained, `ccnet` is responsible for the underlying networking. It needs a directory to store its own configuration and metadata. The directory normally is named `ccnet`.
* seaf-server is responsible for all the other complex jobs (file synchronize, share management). It needs a directory to store its configuration and data. The directory is normally named `seafile-data`.
* seahub is the website front end of seafile server. It's written in Django. If you have any experience with Django, you should know the `syncdb` command must be run to create all the database tables.

So basically these are what the `seafile-admin` script do:

- ensure seafile is already installed and all the python libraries seahub need are installed.
- create the ccnet configuration 
- create the seafile configuration
- run Django `syncdb` command for seahub

Now let's do that step by step.

## Step by Step ##

### The first step ###

To make it easier to migrate or backup your seafile server data, we strongly suggest you create a new directory, and place all the configuration and data under this directory.

In the following part of this section, we suppose you would create a directory `/data/abc-seafile`.

```sh
mkdir /data/abc-seafile
cd /data/abc-seafile
```

This is your directory layout right now:
```
/data
 --abc-seafile/
```

### ccnet-server ###

```sh
$ ccnet-init -c /data/abc-seafile/ccnet --name "foo-seafile" --port 10001 --host 192.168.1.116
```
<table>
  <tr>
    <th>Argument</th>
    <th>Meaning</th>
    <th>Default Value</th>
  </tr>
  <tr>
    <td>-c "ccnet config dir" </td>
    <td>ccnet configuration dir</td>
    <td>~/.ccnet</td>
  </tr>
  <tr>
    <td>--name "server name"</td>
    <td>the name of this server, which would be seen by the clients</td>
    <td>N/A</td>
  </tr>
  <tr>
    <td>--port "ccnet server port"</td>
    <td>The port that ccnet server listens on</td>
    <td>10001</td>
  </tr>
  <tr>
    <td>--host "domain or ip address of this server"</td>
    <td>domain or ip of this server</td>
    <td>N/A</td>
  </tr>
</table>

This is your directory layout now:

```
/data
 --abc-seafile/
   --ccnet/
     --ccnet.conf
```

### seaf-server ###

```sh
$ seaf-server-init --seafile-dir /data/abc-seafile/seafile-data --port 20001
```
<table>
  <tr>
    <th>Argument</th>
    <th>Meaning</th>
    <th>Default Value</th>
  </tr>
  <tr>
    <td>--seafile-dir "seafile-data-dir" </td>
    <td>The directory to store seafile data. Make sure it has enough free space to hold your (future) data.</td>
    <td>~/.ccnet/seafile</td>
  </tr>
  <tr>
    <td>--port "data port"</td>
    <td>the port seaf-server uses to transport data with clients</td>
    <td>N/A</td>
  </tr>
</table>

This is your directory layout now:

```
/data
 --abc-seafile/
   --ccnet/
     --ccnet.conf
   --seafile-data/
     --seafile.conf
```

### Seahub ###

First download seahub:

```sh
cd /data/ # or whatever you like
git clone git@github.com:haiwen/seahub.git
cd /data/seahub
```

Then initialize seafile database:

```sh
export CCNET_CONF_DIR=/data/abc-seafile/ccnet
export PYTHONPATH=/data/seahub/thirdpart
python manage.py syncdb
```

This is your directory layout right now:

```
/data
 --abc-seafile/
   --ccnet/
     --ccnet.conf
   --seafile-data/
     --seafile.conf
   --seahub/
```

### Create a seahub admin account ###

XXX: write how to create a seahub admin

## Start all the componenets ##

Now let's start all the components of Seafile server one by one.

### start ccnet-server ###

```sh
ccnet-server -c /data/abc-seafile/ccnet -d
```

#### start seaf-server and seaf-mon ####

```sh
seaf-server -c /data/abc-seafile/ccnet -d /data/abc-seafile/seafile-data
seaf-mon -c /data/abc-seafile/ccnet -d /data/abc-seafile/seafile-data
```

#### start httpserver ####


```sh
httpserver -c /data/abc-seafile/ccnet -d /data/abc-seafile/seafile-data
```

### start Seahub ###

```sh
cd /data/seahub # or the place you have downloaded seahub
export CCNET_CONF_DIR=/data/abc-seafile/ccnet
export PYTHONPATH=/data/seahub/thirdpart
python manage.py runserver
```

Note: This would only start a basic development server of Django for Seahub, which is enough for a demo or single person usage. To setup a production level web server if you need, see [[Deploy Seahub in a production environment]]

## Done ##

Now open your browser and open `http://YourServerIp:8000`, you can see the seahub website running.