# Clonezilla

Clonezilla  is a tool to clone disks, useful for large deployment

## doing backup/image

Start your server/vps you want to clone in rescue mode with clonezilla

when the distro starts, type at cli

    clonezilla


The easiest way for me was creating a image over ssh. 

So choose "device-image" at first menu and then "ssh-server"

Fill the data to ssh connection, and dont forget to create the folder in remote server first!

Always use fsck, or the 'backup' will fail. 

choose default options for all other items. 


## restore

