# Nginx Webdav


Install nginx full
```
apt -y update
apt-get install nginx-full
```


Turn on gzip on /etc/nginx/nginx.conf . Let it like

```
	gzip on;

	gzip_vary on;
	gzip_proxied any;
	gzip_comp_level 6;
	gzip_buffers 16 8k;
	gzip_http_version 1.1;
	gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
```

Create a webdav conf in /etc/nginx/conf.d/webdav.conf

```
server {
    listen 80;
    listen [::]:80;

    server_name yourDomain;

	index index.html;

	server_name yoursubdomain.domain.com;
	location /storagepath {
            alias /storagepath;
            auth_basic              realm_name;
            # The file containing authorized users
            auth_basic_user_file    /etc/nginx/.credentials.list;

           # dav allowed method
           dav_methods     PUT DELETE MKCOL COPY MOVE;
           # Allow current scope perform specified DAV method
           dav_ext_methods PROPFIND OPTIONS;
    
            # In this folder, newly created folder or file is to have specified permission. If none is given, default is user:rw. If all or group permission is specified, user could be skipped
           dav_access       all:rw;

            # Temporary folder
           client_body_temp_path   /tmp/nginx/client-bodies;
    
            # MAX size of uploaded file, 0 mean unlimited
           client_max_body_size    0;
    
            # Allow autocreate folder here if necessary
            create_full_put_path    on;
	}
    location / {
		return 404;
	}

}
```

Create users on /etc/nginx/.credential.list

```
echo -n 'MyUserName:' | sudo tee -a /etc/nginx/.credentials.list; 
```

Set a password

```
openssl passwd -apr1 | sudo tee -a /etc/nginx/.credentials.list;
```

Create a certificate for your subdomain (its free with certbot)

```
mkdir -p /tmp/nginx/client-bodies
snap install core; snap refresh core
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot
nginx -t
systemctl reload nginx
certbot
```

## Mount on client

Install package

```
apt install davfs2
```

configure /etc/fstab


make a empty storage path
```
mkdir -p /storagepath
```


user mount
```
https://yourdomain.com/storage /storagepath davfs _netdev,noauto,user,uid=youruser,gid=youruser 0 0
```

root only mount
```
https://yourdomain.com/storage /storagepath davfs _netdev,auto,nouser 0 0
```

edit /etc/davfs2/secrets
```
/storagepath 			user	password
```

edit /etc/davfs2/davfs2.conf,  add
```
dav_group users
use_locks 0
```

Add you user(s) to group davfs2

```
adduser username davfs2
```

mount it

```
mount /storagepath
```

your remote files are visible now at /storagepath

## mount manually 

example. Replace with your webdav server and path

    sudo mount -t davfs -o noexec https://nextcloud.example.com/remote.php/webdav/ /<mountpath>/


## umount

Example. Replace with your path


    umount /<youpath>


