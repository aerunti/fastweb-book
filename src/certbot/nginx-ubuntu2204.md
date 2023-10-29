# Certbot

    sudo apt update
    sudo apt install certbot python3-certbot-nginx

configure your domain or subdomain using only port 80 
a good start is the default example of nginx 



    server {
    	listen 80;
    	listen [::]:80;
    
    	server_name example.com;
    
    	root /var/www/example.com;
    	index index.html;
    
    	location / {
    		try_files $uri $uri/ =404;
    	}
    }


personalize it to your case (php, python, proxy, etc )
and check the config

    nginx -t 


Don't forget to configure your domain/subdomiain to server's ip at your dns manager


after checked,  run certbot

    certbot


Certbot will how a list of domains/subdomains to configure.
Follow the instructions. finally restart nginx

    service nginx restart

