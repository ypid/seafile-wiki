## Prepare

1. Install <code>python-flup</code> library. On Ubuntu:
 
    ```
    sudo apt-get install python-flup
    ```

2. Install and enable mod_fastcgi and also enable mod_rewrite. On Ubuntu:

    ```
    sudo apt-get install libapache2-mod-fastcgi
    sudo a2enmod rewrite
    sudo a2enmod fastcgi
    ```

3. Enable apache proxy

    ```
    a2enmod proxy_http
    ```

On Windows, you have to download  [mod_fastcgi-*.dll] (http://fastcgi.com/dist/) first, and put it into the modules directory.

## Deploy Seahub/HttpServer With Apache

Seahub is the web interface of Seafile server. HttpServer is used to handle raw file uploading/downloading through browsers. By default, it listens on port 8082 for HTTP request. 

Here we deploy Seahub using fastcgi, and deploy HttpServer with reverse proxy. We assume you are running Seahub using domain '''www.myseafile.com'''.

First edit your apache config file. Depending on your distro, you will need to add this line to **the end of the file**:

`apache2.conf`, for ubuntu/debian:
```
FastCGIExternalServer /var/www/seahub.fcgi -host 127.0.0.1:8000
```

***

`httpd.conf`, for centos/fedora:
```
FastCGIExternalServer /var/www/html/seahub.fcgi -host 127.0.0.1:8000
```

`httpd.conf`, for Windows:
```
LoadModule fastcgi_module modules/mod_fastcgi-2.4.6-AP22.dll
LoadModule rewrite_module modules/mod_rewrite.so
FastCGIExternalServer e:/seafile-server-1.7.1/seahub/seahub.fcgi -host 127.0.0.1:8000

```


Note, `seahub.fcgi` is just a placeholder, you don't need to actually have this file in your system.

Second, modify Apache config file:
(`site-enabled/000-default`) for ubuntu/debian
(`vhost.conf`) for centos/fedora

```
<VirtualHost *:80>
  ServerName www.myseafile.com
  DocumentRoot /var/www
  Alias /media  /home/user/haiwen/seafile-server-1.6.1/seahub/media         

  RewriteEngine On

  #
  # seafile httpserver
  #
  ProxyPass /seafhttp http://127.0.0.1:8082
  ProxyPassReverse /seafhttp http://127.0.0.1:8082
  RewriteRule ^/seafhttp - [QSA,L]

  #
  # seahub
  #
  RewriteRule ^/(media.*)$ /$1 [QSA,L,PT]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^(.*)$ /seahub.fcgi$1 [QSA,L,E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
</VirtualHost>
```

## Modify ccnet.conf and seahub_setting.py

### Modify ccnet.conf

You need to modify the value of <code>SERVICE_URL</code> in <code>/data/haiwen/ccnet/ccnet.conf</code>
to let Seafile know the domain you choose.

<pre>
SERVICE_URL = http://www.myseafile.com
</pre>

Note: If you later change the domain assigned to seahub, you also need to change the value of  <code>SERVICE_URL</code>.

### Modify seahub_settings.py

You need to add a line in <code>seahub_settings.py</code> to set the value of `HTTP_SERVER_ROOT`

```python
HTTP_SERVER_ROOT = 'http://www.myseafile.com/seafhttp'
```

## Start Seafile and Seahub

<pre>
sudo service apache2 restart
./seafile.sh start
./seahub.sh start-fastcgi
</pre>

## Notes when Upgrading Seafile Server

When [[upgrading seafile server]], besides the normal steps you should take, there is one extra step to do: '''Update the path of the static files in your Nginx/Apache configuration'''. For example, assume your are upgrading seafile server 1.3.0 to 1.4.0, then:

```
  Alias /media  /home/user/haiwen/seafile-server-1.4.0/seahub/media
```