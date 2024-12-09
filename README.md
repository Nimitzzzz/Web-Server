# Leaerning Management System Web Service
ANDI AHMAD FAUZAN
23.83.0970
23TK01


# YANG SAYA GUNAKAN
- nginx / 1.18.0 
- Flask / 3.1.0
- MySQL / 8.0.40
- Samba / 4.15.13
- Gunicorn / 23.0.0
- phpMyAdmin /


# OS
Distributor  : Ubuntu <br>
Description  : Ubuntu 22.02.5 LTS <br>
Release      : 22.04 <br>
Codename     : Jammy <br>

# Membuat Struktur Direktori
- project/
  - templates
    - index.html
  - static/
    - css/
    - js/
    - images/
  - __init__py
  - routes.py
  - models.py
  - config.py
- app/
- run.py
- requirements.txt

# Setup MySQL
```bash
sudo mysql -u root -p

#Buat database dan user baru
CREATE DATABASE lms_db;
CREATE USER 'lms_user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON lms_db.* TO 'lms_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

# Membuat Virtual Environment dan flask
```bash
#Membuat direktori parent
mkdir ~/project

#Membuat virtual environment
python3 -m venv venv

#Mengaktifkan virtual environment
source venv/bin/activate

#Installasi Flask dan Library lainnya
pip install flask flask_sqlalchemy pymysql

#Tambahkan direktoridalam direktori parent, app(flask), templates & static (frontend)
mkdir app 
mkdir templates static

#Membuat file didalam app
touch __init__.py routes.py models.py config.py
```

# Konfigurasi Flask
```bash
#Didalam file app/__init__.y
from flask import Flask
from app.routes import main
from flask_sqlalchemy import SQLAlchemy
db = SQLAlchemy()

def create_app():
    app = Flask(__name__)
    app.config.from_object('app.config.Config')
    db.init_app(app)
    app.register_blueprint(main)
    return app

#Didalam file app/config.py
class Config:
    SECRET_KEY = '1'
    SQLALCHEMY_DATABASE_URI = 'mysql+pymysql://lms_user:password@localhost/lms_db'
    SQLALCHEMY_TRACK_MODIFICATIONS = False

#Didalam file app/routes.py
from flask import Blueprint, render_template

main = Blueprint('main', __name__)

@main.route('/')
def index():
    return render_template('index.html')

#Didalam app/templates/index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LMS Home</title>
</head>
<body>
    <h1>Selamat Datang Di Web Saya</h1>
</body>
</html>
```

# Konfigurasi file run.py & menjalankannya
```bash
from app import create_app
app = create_app()

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)

#Save dan keluar config lalu jalankan
python run.py
```

# Setup NginX
### Menggunakan gunicorn untuk menjalankan flask
```bash
gunicorn -w 3 -b 127.0.0.1:5000 run:app
```

### Konfigurasi Nginx
```bash
sudo nano /etc/nginx/sites-available/lms

#Konfigurasi didalam NginX
server {
    listen 80;
    server_name 192.168.100.177;

    root /home/sateayam/project/app/templates;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    error_log /var/log/nginx/lms_error.log;
    access_log /var/log/nginx/lms_access.log;
}

```

### Konfigurasi pada nginx.conf
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
```
### Mengaktifkan konfigurasi nginx
```bash
sudo ln -s /etc/nginx/sites-available/lms /etc/nginx/sites-enabled
sudo systemctl restart nginx
```
