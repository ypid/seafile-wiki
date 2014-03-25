## Components of Seafile Server

Seafile server comprises of the following services

* Ccnet daemon (ccnet for client side or ccnet-server for server side)：networking service daemon. In our initial design, Ccnet should work like a traffic bus. All the network traffic between client and server and internal traffic between different components should go through Ccnet. But later, we find that file transfer is better go between seafile daemon component directly.
* Seafile daemon：data service daemon
* Seahub：the website. Seafile server package contains a light-weight Python HTTP server `gunicorn` that serve the website. Seahub runs as an app inside gunicorn.
* HttpServer: handles raw file upload/download for Seahub. Because gunicorn is poor at handling large files, so we write this "HttpServer" in C programming language to serve raw file upload/download.
* Controller: monitors ccnet and seafile daemons, restart them if necessary

**The picture below shows how Seafile desktop client syncs files with Seafile server**:

[[images/seafile-sync-arch.png]]

<br/>

**The picture below shows how Seafile mobile client interacts with Seafile server**:

[[images/mobile-arch.png]]

<br/>

**The picture below shows how Seafile mobile client interacts with Seafile server if the server is configured behind Nginx/Apache**:

[[images/mobile-nginx-arch.png]]