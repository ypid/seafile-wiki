### Before reading

Note: We have pre-compiled Seafile Server tarball, which supports these Linux distributions:

* CentOS 6
* Debian 6
* Ubuntu 12.04 Server

If your distribution is in the list above, we suggest you use the pre-compiled package, because we have written helper shell scripts to help you deploy seafile server. The next part of this article are just for those who compile Seafile server from source.

## Deploy Seafile Server on Linux

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



### Initialization

* ccnet-server
* seafile-server
* seahub

#### ccnet-server

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

#### seaf-server

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

#### Seahub

Seahub is website server of Seafile. It's written in the [django](http://djangoproject.com) framework.

Seahub requires Python 2.7 installed on your server, and it depends on the following python modules:  

* [django 1.3.1](https://www.djangoproject.com/download/1.3.1/tarball/)
* [djblets](https://github.com/djblets/djblets/tarball/release-0.6.14)
* python-sqlite3
* python-simplejson
