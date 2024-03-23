# nginxSSL
Easy SSL w/Nginx

## Instructions
Tested with Debian 12 VPS
```sh
sudo apt update
sudo apt upgrade
sudo apt install certbot python3-certbot-nginx nginx ufw
sudo nano /etc/nginx/nginx.conf
```
Input: (Credit: https://docs.titaniumnetwork.org/guides/nginx)
```nginx
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 1024;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    map_hash_bucket_size 128;

    sendfile on;
    tcp_nopush on;

    tcp_nodelay on;

    reset_timedout_connection on;

    access_log off;
    error_log off;

    server {
        listen 80 default_server;
        listen [::]:80 default_server;
        server_name subdomain.domain.tld; # Replace with your domain

        location / { # Replace / with path
                proxy_pass http://127.0.0.1:3000; # Change 3000 to your port
        }
    }
}
```
Save and exit
```sh
sudo systemctl reload nginx
sudo ufw allow 'Nginx Full'
sudo ufw delete 'Nginx HTTP'
sudo certbot --nginx -d subdomain.domain.tld
```
