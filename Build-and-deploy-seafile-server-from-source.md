## Build Seafile Server ##

### Preparation ###

The following list is all the libraries you need to install on your machine. __You should install all of them before you build seafile__.

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
* libevhtp (https://github.com/ellzey/libevhtp/archive/1.1.6.zip)

libzdb relies on two packages: `re2c` and `flex`  
libevhtp needs to be built by `cmake`

**Seahub** is the web front end of Seafile. It's written in the [Django](http://djangoproject.com) framework. Seahub requires Python 2.6(2.7) installed on your server, and it needs the following python libraries:  

* [django 1.3](https://www.djangoproject.com/download/1.3.1/tarball/)
* [djblets](https://github.com/djblets/djblets/tarball/release-0.6.14)
* sqlite3 
* simplejson (python-simplejson)
* PIL (aka. python imaging library, python-image)
* gunicorn

Before continue, make sure you have all these libraries available in your system.

### Building ###

To build Seafile Server, you need first build the latest version of **libsearpc** and **ccnet**.

#### libsearpc ####

```sh
wget https://github.com/downloads/haiwen/libsearpc/libsearpc-latest.tar.gz
tar xzf libsearpc-latest.tar.gz
cd libsearpc-${VERSION}
./configure
make
make install
```

#### ccnet ####

```sh
wget https://github.com/downloads/haiwen/ccnet/ccnet-latest.tar.gz
tar xzf ccnet-latest.tar.gz
cd ccnet-${VERSION} # VERSION is the latest version number
./configure --disable-client --enable-server
make
make install
```

#### seafile ####

```sh
wget https://github.com/downloads/haiwen/seafile/seafile-latest.tar.gz
tar xzf seafile-latest.tar.gz
cd seafile-${VERSION}
./configure --disable-client --enable-server --enable-httpserver
make
make install
```

## Deploy Seafile Server ##

### Components of the Seafile server

The Seafile server consists of the following components:

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

### Configurations of all components ###

* **ccnet** needs a directory to store its own configuration and metadata, normally is named `ccnet`.
* **seaf-server** nees a directory to store its configuration and data, normally named `seafile-data`.
* **seahub** is written in Django. If you have any experience with Django, you should know the `syncdb` command must be run to create all the database tables.
* An **admin account** has to be created, so that you, the admin, can login with this account to manage the server.

### Create Configuration ###

These are the essential steps to create the configuration:

- ensure seafile is already installed and all the python libraries seahub need are installed.
- create the ccnet configuration with the `ccnet-init` program
- create the seafile configuration with `seaf-server-init` program
- run Django `syncdb` command for seahub
- create an admin account for the seafile server

To create the configurations, you can **choose either**:

* use the seafile-admin script
* [[create server configuration by hand]]



#### Create Configurations with the seafile-admin script ####

`seafile-admin` should have been installed to system path after you have built and installed Seafile from source. It has three subcommands:
```
Usage: seafile-admin <operation>

    setup       guide you to setup the seafile server
    start       start the seafile server
    stop        stop the seafile server
```

##### Create a new directory for all the configuration and data

To make it easier to migrate or backup your seafile server data, we strongly suggest you create a new directory, and place all the configuration and data under this directory. 

```sh
mkdir /data/abc-seafile
cd /data/abc-seafile
```

##### Download seahub to this directory

```sh
wget https://github.com/downloads/haiwen/seahub/seahub-latest.tar.gz
tar xzf seahub-latest.tar.gz
mv seahub-${VERSION} seahub
```

##### Run `seafile-admin setup` to create all the configuration

```sh
seafile-admin setup
```

The script would ask you a series of questions, and finally create the configuration for you.

<table>
  <tr>
    <th>Name</th><th>Usage</th><th>Default</th><th>Requirement</th>
  </tr>
  <tr>
    <td>server name</td>
    <td>The name of the server that would be shown on the client</td>
    <td></td>
    <td>3 ~ 15 letters or digits</td>
  </tr>
  <tr>
    <td>ip or domain</td>
    <td>The ip address or domain name of the server</td>
    <td></td>
    <td>Make sure to use the right ip or domain, or the client would have trouble connecting it</td>
  </tr>
  <tr>
  <td>ccnet port</td>
  <td>the tcp port used by ccnet</td>
  <td>10001</td>
  <td></td>
  </tr>
  <tr>
    <td>seafile port</td>
    <td>tcp port used by seafile</td>
    <td>12001</td>
    <td></td>
  </tr>
  <tr>
    <td>admin email</td>
    <td>Email address of the admin account</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>admin password</td>
    <td>password of the admin account</td>
    <td></td>
    <td></td>
  </tr>
</table>

This is a screenshot of the `seafile-admin setup` command:
[[images/seafile-admin-1.png]]

And a screenshot after setup is finished successfully:
[[images/seafile-admin-2.png]]

At this time, the directory layout would be like this:
```
/data
 --abc-seafile/
   --ccnet/
     --ccnet.conf
   --seafile-data/
     --seafile.conf
   --seahub/
```

##### Start the Seafile server

After configuration successfully created, run `seafile-admin start` to start the all components of Seafile

```sh
seafile-admin start
```

At this moment, all the components should be running and seahub can be visited at **http://yourserver-ip-or-domain:8000**

##### Stop the Seafile server

To stop Seafile server, run `seafile-admin stop` to stop all components of Seafile

```sh
seafile-admin stop
```



## Problems Report ##

If you encounter any problem when building/deploying Seafile, please leave us a message or open an issue.