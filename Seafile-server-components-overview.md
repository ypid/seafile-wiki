## Components of Seafile Server

Seafile server comprises of the following services

* Ccnet：networking service daemon
* Seafile server：data service daemon
* Seahub：the website. Seafile server package contains a light-weight Python HTTP server `gunicorn` that serve the website. Seahub runs as an app inside gunicorn.
* HttpServer: handles raw file upload/download for Seahub. Because gunicorn is poor in handling large files, so we write this "HttpServer" in C programming language to serve raw file upload/download.
* Controller: monitors ccnet and seafile daemons, restart them if necessary

[[images/server-arch.png]]