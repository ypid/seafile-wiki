## Components of Seafile Server

Seafile server comprises of the following services

* Ccnet daemon：networking service daemon. In our initial design, Ccnet should work like a traffic bus. All the network traffic between client and server and internal traffic between different components should go through Ccnet. But later, we find that file transfer is better go between seafile daemon component directly.
* Seafile daemon：data service daemon
* Seahub：the website. Seafile server package contains a light-weight Python HTTP server `gunicorn` that serve the website. Seahub runs as an app inside gunicorn.
* HttpServer: handles raw file upload/download for Seahub. Because gunicorn is poor in handling large files, so we write this "HttpServer" in C programming language to serve raw file upload/download.
* Controller: monitors ccnet and seafile daemons, restart them if necessary

[[images/server-arch.png]]