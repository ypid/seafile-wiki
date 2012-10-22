## Build ##

### Preparation ###

The following list is what you need to install on your development machine. __You should install all of them before you build seafile__.

Package names are according to Ubuntu 11.10. For other Linux distros, please find their corresponding names yourself.

* libevent-dev ( 2.0 or later )
* libcurl4-openssl-dev  (1.0.0 or later)
* libglib-2.0 ( 2.28 or later)
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

## Deploy ##

To deploy Seafile server, we need first know about its components.

### Components of the Seafile server

The Seafile server consists of the following parts:

<table>
  <tr>
    <th>Process Name</th><th>Functionality</th>
  </tr>
  <tr>
    <td>ccnet-server</td><td>underlying networking, peer management</td>
  </tr>
  <tr>
    <td>seaf-server</td><td>data management</td>
  </tr>
  <tr>
    <td>seaf-monitor</td><td>monitor statistics</td>
  </tr>
  <tr>
    <td>Seahub</td><td>website component of Seafile server</td>
  </tr>
  <tr>
    <td>httpserver</td><td>handling raw file upload/download for Seahub</td>
  </tr>
</table>

### Create Configuration ###

Now let's create configuration for our components:

* ccnet-server
* seafile-server
* seahub

#### ccnet-server ####

```sh
$ ccnet-init -c ~/.ccnet --name "abc-seafile" --port 10001 --host 192.168.1.116
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

#### seaf-server ####

```sh
$ seaf-server-init --seafile-dir ~/seaf-server-data --port 20001
```
<table>
  <tr>
    <th>Argument</th>
    <th>Meaning</th>
    <th>Default Value</th>
  </tr>
  <tr>
    <td>--seafile-dir "seafile-data-dir" </td>
    <td>the directory to store seafile data</td>
    <td>~/.ccnet/seafile</td>
  </tr>
  <tr>
    <td>--port "data port"</td>
    <td>the port seaf-server uses to transport data with clients</td>
    <td>N/A</td>
  </tr>
</table>

#### Seahub ####

Seahub is website server of Seafile. It's written in the [django](http://djangoproject.com) framework.
Seahub requires Python 2.7 installed on your server, and it depends on the following python modules:  

* [django 1.3.1](https://www.djangoproject.com/download/1.3.1/tarball/)
* [djblets](https://github.com/djblets/djblets/tarball/release-0.6.14)
* python-sqlite3
* python-simplejson

```sh
export CCNET_CONF_DIR=${CCNET_CONF_DIR}
python manage.py syncdb
```

### Start all the componenets ###

#### start ccnet-server ####
#### start seaf-server ####
#### start httpserver ####
#### start seahub ####

## Problems Report ##

If you encounter any problem when building Seafile, please leave us a message or open an issue.