# Seafile data backup #

If you setup seafile server according to our manual, you should have a directory layout like:

    haiwen       # Replace the name with your organization name
      --seafile-server-1.2.0 # untar from seafile package
      --seafile-data   # seafile configuration and data (if you choose the default)
      --seahub-data    # seahub data
      --ccnet          # ccnet configuration and data 
      --seahub.db      # sqlite3 database used by seahub
      --seahub_settings.py # optional config file for seahub

Seafile data are all stored in the `haiwen` directory, so just back up the whole directory.

We use rsync on our backup machine to pull the directory on machine A. Command looks like:

    mkdir /backup
    rsync -azv --delete user@A:/path/to/haiwen /backup/

Also you might want to set up a script framework that calls such a command via `cron`. Rule looks like:

     0 3 * * * rsync -azv --delete user@A:/path/to/haiwen /backup/

This will perform backup operation on 3:00 am everyday.     
