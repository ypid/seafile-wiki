## Build Seafile Server ##

**Warning: not finished yet!**

### Preparation ###

The following list is what you need to install on your development machine. __You should install all of them before you build seafile__.

Package names are according to Ubuntu 12.04. For other Linux distros, please find their corresponding names yourself.

* libevent-dev ( 2.0 or later )
* libcurl4-openssl-dev  (1.0.0 or later)
* libglib2.0-dev ( 2.28 or later)
* uuid-dev
* intltool ( 0.40 or later)
* libsqlite3-dev (3.7 or later)
* python2.7-dev 
* libmysqlclient-dev (5.5 or later)

The following libraries need to be compiled from source.

* libzdb (http://www.tildeslash.com/libzdb/dist/libzdb-2.10.5.tar.gz)
* libevhtp (https://github.com/downloads/ellzey/libevhtp/libevhtp-0.3.0.tar.gz)

libzdb relies on two packages: `re2c` and `flex`  
libevhtp needs to be built by `cmake`

### Building ###

To build Seafile Server, you need first build the latest version of **libsearpc** and **ccnet**.

#### libsearpc ####

* [download](https://www.github.com/haiwen/libsearpc/downloads) the latest version tarball , and untar it
* cd libsearpc-${VERSION}
* ./configure
* make
* make install

#### ccnet-server ####

* [download](https://www.github.com/haiwen/ccnet/downloads) the latest version tarball , and untar it
* cd ccnet-${VERSION}
* ./configure --enable-server --disable-client
* make
* make install

#### seafile ####

* [download](https://www.github.com/haiwen/seafile/downloads) the latest version tarball , and untar it
* cd seafile-${VERSION}
* ./configure --enable-server --enable-httpserver
* make
* make install

## Deploy Seafile Server ##

To deploy Seafile server, we need first know about its components.

### Components of the Seafile server

The Seafile server consists of the following parts:

<table>
  <tr>
    <th>Process Name</th><th>Functionality</th>
  </tr>
  <tr>
    <td>ccnet-server</td><td>underlying networking</td>
  </tr>
  <tr>
    <td>seaf-server</td><td>data management</td>
  </tr>
  <tr>
    <td>seaf-monitor</td><td>monitoring seafile statistics</td>
  </tr>
  <tr>
    <td>Seahub</td><td>website front-end of Seafile server</td>
  </tr>
  <tr>
    <td>httpserver</td><td>handles raw file upload/download for Seahub</td>
  </tr>
</table>

Seahub is website server of Seafile. It's written in the [Django](http://djangoproject.com) framework.
Seahub requires Python 2.7 installed on your server, and it depends on the following python libraries:  

* [django 1.3](https://www.djangoproject.com/download/1.3.1/tarball/)
* [djblets](https://github.com/djblets/djblets/tarball/release-0.6.14)
* sqlite3
* simplejson
* PIL (aka. python imaging library)
* gunicorn

Before continue, make sure you have all these libraries available in your system.

### Create Configuration ###

To create the configurations, you can choose either:

* Use the seafile-admin script (Recommended)
* Do all the configuration by hand

#### Create Configurations with the seafile-admin script ####

To help you create the configuration, Seafile includes a script called **seafile-admin**, which should have been installed to system path after you have built and installed Seafile from source.

To use the script:

* Create a new directory for all the configuration and data
* Enter the directory, and download seahub, the website frontend of seafile to this directory.
* Run `seafile-admin setup` to create all the configuration
* After configuration successfully created, run `seafile-admin start` to start the all components of Seafile.
* When needed, run `seafile-admin stop` to stop all components of Seafile.

```sh
mkdir /data/abc-seafile
cd /data/abc-seafile
git clone git@github.com:haiwen/seahub.git
seafile-admin setup # it will guide you step by step
seafile-admin start
seafile-admin stop
```

#### Create Configurations By Hand ####

Normally, you should use the `seafile-admin` script to setup/manage your seafile server. Nevertheless, if you want to take a look at what the script does under the hood, just read on.

We suggest you create a new directory to store all the seafile configuration and data. In the following part of this article, we suppose you would create a directory `/data/abc-seafile` to store seafile data.

The first setup:

```sh
mkdir /data/abc-seafile # or whatever name you like
cd /data/abc-seafile
```

##### ccnet-server #####

```sh
$ ccnet-init -c /data/abc-seafile/ccnet --name "abc-seafile" --port 10001 --host 192.168.1.116
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

##### seaf-server #####

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

##### Seahub #####

First download seahub

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

### Start all the componenets ###

#### start ccnet-server ####

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

#### start Seahub ####

```sh
cd /data/seahub # or the place you have downloaded seahub
export CCNET_CONF_DIR=/data/abc-seafile/ccnet
export PYTHONPATH=/data/seahub/thirdpart
python manage.py runserver
```

Note: This would only start a basic development server of Django for Seahub, which is enough for a demo or single person usage. To setup a production level web server if you need, see [[Deploy Seahub in a production environment]]

### Done ###

Now open your browser and open `http://YourServerIp:8000`, you can see the seahub website running.

## Problems Report ##

If you encounter any problem when building/deploying Seafile, please leave us a message or open an issue.