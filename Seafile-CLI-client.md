#Seafile client for a Cli server

##Installation (debian based)

1. download & install client
  1. get latest url @ http://www.seafile.com/en/download/
  1. then 
```sh
cd /usr/local/src
wget http://seafile.googlecode.com/files/seafile_1.5.2_amd64.deb  #download it 
gdebi seafile_1.5.2_amd64.deb #install it with the required dependencies
```

1. initialise & daemonize the client
```sh
# choose a folder where to store the seafile client settings e.g ~/.seafile-client
mkdir ~/.seafile-client            #create the settings folder
seaf-cli init -d ~/.seafile-client #initialise seafile client with this folder
seaf-cli start # start seaf-cli daemon maybe this can or should be added to /etc/rc.local ? 
```

1. download a library from a server
  1. retrieve the library id by browsing on the server -> it's in the url after `/repo/`
  1. then
```sh
seaf-cli download -l "the id of the library" -s  "the url + port of server" -d "the folder where the library folder will be downloaded" -u "username on server" [-p "password"]
seaf-cli status  # check status of ongoing downloads
# Name  Status  Progress
# Apps    downloading     9984/10367, 9216.1KB/s
```

    Note, if you not supply the password parameter in the command, it will be asked later, which is more safe.

1. download a library from a server and sync with an existing folder.
```sh
# This is the same as download : replace download by sync 
seaf-cli sync -l "the id of the library" -s  "the url + port of server" -d "the folder where the library folder will be downloaded" -u "username on server" -p "password"
```

1. rejoice


##Uninstallation
```sh
seaf-cli stop
rm -rf ~/.seafile-client
rm -rf ~/.ccnet   #note this should note be erased if you run the server on the same host
apt-get remove seafile
apt-get autoremove
```


##Man documentation

seaf-cli is command line interface for seafile client.

Subcommands:

    init:           create config files for seafile client
    start:          start and run seafile client as daemon
    stop:           stop seafile client
    list:           list local liraries
    status:         show syncing status
    download:       download a library from seafile server
    sync:           synchronize an existing folder with a library in
                        seafile server
    desync:         desynchronize a library with seafile server


Detail
======

Seafile client stores all its configure information in a config dir. The default location is `~/.ccnet`. All the commands below accept an option `-c <config-dir>`.

init
----
Initialize seafile client. This command initializes the config dir. It also creates sub-directories `seafile-data` and `seafile` under `parent-dir`. `seafile-data` is used to store internal data, while `seafile` is used as the default location put downloaded libraries.

    seaf-cli init [-c <config-dir>] -d <parent-dir>

start
-----
Start seafile client. This command start `ccnet` and `seaf-daemon`, `ccnet` is the network part of seafile client, `seaf-daemon` manages the files.

    seaf-cli start [-c <config-dir>]

stop
----
Stop seafile client.

    seaf-cli stop [-c <config-dir>]


Download
--------
Download a library from seafile server

    seaf-cli download -l <library-id> -s <seahub-server-url> -d <parent-directory> -u <username> [-p <password>]


sync
----
Synchronize a library with an existing folder.

    seaf-cli sync -l <library-id> -s <seahub-server-url> -d <existing-folder> -u <username> [-p <password>]

desync
------
Desynchronize a library from seafile server

    seaf-cli desync -d <existing-folder>
