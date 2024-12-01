### Web Service

ANDI AHMAD FAUZAN
23.83.0970
23TK01

### VERSI & SPESIFIKASI SERVER

- nginx/1.18.0 (Ubuntu)

### OS

Distributor  : Ubuntu <br>
Description  : Ubuntu 22.02.5 LTS <br>
Release      : 22.04 <br>
Codename     : Jammy <br>

```bash
#Install NginX
sudo apt install nginx
sudo apt install nginx

#Memeriksa status
sudo systemctl status nginx
```

### Melihat paket NginX
```bash
sudo ufw app list
```

### Mengizinkan http Nginx
```bash
sudo ufw allow 'Nginx HTTP'
```

## cek ufw status
```bash
sudo ufw status
```

### setting server blocks
```bash
sudo mkdir -p /var/www/nimitzzzzserver/html
sudo chown -R $USER:$USER /var/www/
nimitzzzzserver
sudo chmod -R 755 /var/www/nimitzzzzserver
nano /var/nimitzzzzserver/html/index.html
```

### sample
```bash
<html>
    <head>
        <title>Hai, selamat datang di nimitzzzz server</title>
    </head>
    <body>
        <h1>Aku Sayang Kamu</h1>
    </body>
</html>

```
### Konfigurasi NginX
untuk melakukan konfigurasi menggunakan nano
```bash
sudo nano /etc/nginx/sites-available/nimitzzzzserver
```
```bash
server {
        listen 80;
        listen [::]:80;

        # SSL configuration
        #
        # listen 443 ssl default_server;
        # listen [::]:443 ssl default_server;
        #
        # Note: You should disable gzip for SSL traffic.
        # See: https://bugs.debian.org/773332
        #
        # Read up on ssl_ciphers to ensure a secure configuration.
        # See: https://bugs.debian.org/765782
        #
        # Self signed certs generated by the ssl-cert package
        # Don't use them in a production server!
        #
        # include snippets/snakeoil.conf;

        root /var/www/nimitzzzzserver/html;

        # Add index.php to the list if you are using PHP
         index index.html index.htm index.nginx-debian.html;

        server_name nimitzzzzserver.my.id www.nimitzzzzserver.my.id;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }
        error_log /var/log/nginx/error.log;
        access_log /var/log/nginx/access.log;
        # pass PHP scripts to FastCGI server
        #
        #location ~ \.php$ {
        #       include snippets/fastcgi-php.conf;
        #
        #       # With php-fpm (or other unix sockets):
        #       fastcgi_pass unix:/run/php/php7.4-fpm.sock;
        #       # With php-cgi (or other tcp sockets):
        #       fastcgi_pass 127.0.0.1:9000;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        }

```
menjalankan file dengan membuat link di sites-enabled
```bash
sudo ln -s /etc/nginx/sites-available/nimitzzzzserver /etc/nginx/sites-enabled/
```
edit kofigurasi pada /etc/nginx/nginx.conf
```bash
sudo nano /etc/nginx/nginx.conf
```
```bash
...
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
...
```
test nginx
```bash
sudo nginx -t
```
restart nginx
```bash
sudo systemctl restart nginx
```



