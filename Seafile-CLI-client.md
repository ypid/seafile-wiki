#Seafile client for a Cli server

##Installation

1. Download the client
```sh
wget http://seafile.googlecode.com/files/seafile-cli_1.5.3_x86-64.tar.gz 
tar xzf seafile-cli_1.5.3_x86-64.tar.gz
```

1. Initialise & daemonize the client
```sh
cd seafile-cli_1.5.3
# choose a folder where to store the seafile client settings e.g ~/.seafile-client
mkdir /tmp/seaf-cli-data            # create the settings folder
./seaf-cli init -d /tmp/seaf-cli-data # initialise seafile client with this folder
./seaf-cli start
```

1. Install seafile in your environment
```sh
#link the full path of the exectuable
ln -s `readlink -f seaf-cli` /usr/bin/
```

1. Download a library from a server
  1. retrieve the library id by browsing on the server -> it's in the url after `/repo/`
  1. then
```sh
seaf-cli download -l "the id of the library" -s  "the url + port of server" -d "the folder where the library folder will be downloaded" -u "username on server" [-p "password"]
seaf-cli status  # check status of ongoing downloads
# Name  Status  Progress
# Apps    downloading     9984/10367, 9216.1KB/s
```

    Note: if you not supply the password parameter in the command, it will be asked later, which is more safe.

    Example: `seaf-cli download -l 0536c006-8a43-449e-8718-39f12111620d -s http://cloud.seafile.com -d /tmp -u freeplant@test.com`

1. Download a library from a server and sync with an existing folder.
```sh
# This is the same as download : replace download by sync 
seaf-cli sync -l "the id of the library" -s  "the url + port of server" -d "the folder where the library folder will be downloaded" -u "username on server" -p "password"
```

1. :sparkles: rejoice :sparkles:

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