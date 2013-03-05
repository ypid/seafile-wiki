**Note:** Do not forget to update your Nginx/Apache config file if you deploy seafile with them.

This page is for users who use the pre-compiled seafile server package. If you [build seafile server from source](Build and Deploy Seafile Server from source), please read the **Upgrading Seafile Server** section on that page, instead of this one.
 
##Full example to upgarde from 1.[2-3-4].[1-5] to 1.5

1. Current state of server folder 
```sh
# tree -L 1 .
.
├── app
├── ccnet
├── previous
├── seafile-data
├── seafile-server-1.4.0
├── seafile-server-1.4.5
├── seahub-data
├── seahub.db
├── seahub_settings.py
└── seahub_settings.pyc
```

1. Download & extract new server package
```sh
wget http://seafile.googlecode.com/files/seafile-server_1.5.0_x86-64.tar.gz #download archive
aunpack seafile-server_1.5.0_x86-64.tar.gz      #unpack archive using atool
mv seafile-server_1.5.0_x86-64.tar.gz previous  #dispose of archive
```

1. Stop current version server
```sh
seafile-server-1.4.5/seahub.sh stop  
seafile-server-1.4.5/seafile.sh stop
```

1. Upgrade version & start new version
```sh
seafile-server-1.5.0/upgrade/upgrade_1.4_1.5.sh   #upgrade 
#note : you can use upgrade_1.2_1.3.sh or upgrade_1.3_1.4.sh if you are upgrading from 1.2 or 1.3
seafile-server-1.5.0/seafile.sh start
seafile-server-1.5.0/seahub.sh start
```