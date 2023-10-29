# Rsync


Backup ignoring gitignore files

Create a file backup.sh with this content

    #!/usr/bin/env bash
    rsync -ahP --exclude-from="$(git -C $1 ls-files --exclude-standard -oi --directory > /tmp/excludes; echo /tmp/excludes)" $1 $2

make it executable:

    chmod +x backup.sh

save it and call it informing src and destination

    bash backup.sh <SRC> <DST>

you can put it in your /usr/local/bin folder and run anywhere 

    cp backup.sh /usr/local/bin/backup

run from any place you computer/server. remember <SRC> and <DST> should follow rsync conventions 

    backup <SRC> <DST>

