### Web Service

ANDI AHMAD FAUZAN
23.83.0970
23TK01

### VERSI & SPESIFIKASI SERVER

- ginx web server
- squid server
- load balancer harproxy

### OS

Distributor  : Ubuntu <br>
Description  : Ubuntu 22.02.5 LTS <br>
Release      : 22.04 <br>
Codename     : Jammy <br>

'''bash
# Install NginX
sudo apt install nginx
sudo apt install nginx

# Memeriksa status
sudo systemctl status nginx
'''

### Melihat paket NginX
'''bash
sudo ufw app list
'''

### Mengizinkan http Nginx
'''bash
sudo ufw allow 'Nginx HTTP'
'''

## cek ufw status
bash'''
sudo ufw status
'''

### setting server blocks



