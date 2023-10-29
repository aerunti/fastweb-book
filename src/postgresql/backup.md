# Backup and restore database 

ps: cwa

## Simple Example to Backup a database

    sudo -u postgres pg_dump <mydbname> > <mydbname>.sql

where <mydbname> is the name of database you want do backup and restore after.

## restore a database

    sudo -u postgres psql < <mydbname>.sql
