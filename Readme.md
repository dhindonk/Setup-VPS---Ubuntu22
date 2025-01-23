# Panduan Setup Awal VPS Ubuntu 22.04

Dokumen ini berisi langkah-langkah *setup* awal yang disarankan setelah pertama kali mengakses VPS Ubuntu 22.04.

## Koneksi Awal

Untuk terhubung ke VPS Anda, gunakan perintah SSH berikut di terminal lokal Anda:

```bash
ssh username@ippublic
```
contoh : 
ssh dhin@102.893.092.1

## Install Mysql/mariadb
```bash
sudo apt-get update sudo apt install mariadb-server mariadb-client
```
```bash
sudo mysql_secure_installation
```
Enter current password for root: (Enter your SSH root user password)
- Switch to unix_socket authentication [Y/n]: Y
- Change the root password? [Y/n]: Y
It will ask you to set new MySQL root password at this step. This can be different from the SSH root user password.
- Remove anonymous users? [Y/n] Y
- Disallow root login remotely? [Y/n]: N
This is set as N because we might want to access the database from a remote server for using business analytics software like Metabase / PowerBI / Tableau, etc. 
- Remove test database and access to it? [Y/n]: Y
- Reload privilege tables now? [Y/n]: Y

## Connect Mysql/mariadb & Testing
![image](https://github.com/user-attachments/assets/7d9bbe58-c6ad-4d50-901d-20b4e5fc573c)

![image](https://github.com/user-attachments/assets/40ac8cbf-1bad-4d9c-9acd-00506f8288a8)

## Install Web server Nginx
```bash
sudo apt install nginx
```
## Install PHP
```bash
sudo dpkg -l | grep php | tee packages.txt
```
```bash
sudo add-apt-repository ppa:ondrej/php
```
```bash
sudo apt update
```
```bash
sudo apt install php8.2 php8.2-cli php8.2-{bz2,curl,mbstring,intl}
```
```bash
sudo apt install php8.2-fpm
```
```bash
sudo apt install php8.2-mysql
```
```bash
sudo apt install php-curl php-json php-common php-zip php-gd php-xml php-pear php-bcmath
```
```bash
sudo apt install curl git unzip
```
## Install Composer
```bash
curl -sS https://getcomposer.org/installer -o composer-setup.php
```
```bash
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```
## Clone Project dari Github

```bash
cd /var/www
```
```bash
sudo git clone git@github.com:link_repo
```
* Ganti link_repo dengan link project di github
  
## Setting .env
```bash
cd nama_folder_project
```
```bash
sudo cp .env.example .env
```

## Edit bagian access database
ada 2 cara yang direkomendasikan:
-
#1
```bash
sudo vim .env
```
#2
```bash
sudo nano .env
```

## Composer Install
```bash
sudo composer install
```
```bash
sudo php artisan key:generate
```
```bash
sudo chmod -R 775 storage bootstrap/cache
```
```bash
sudo chown -R www-data:www-data storage bootstrap/cache
```

## Konfigurasi domain
[ Foto ] 
```bash
sudo vim /etc/nginx/sites-available/master.conf
```

# Sesuaikan konten dibawah lalu Copy paste 
```bash
server {
 listen 80;
 server_name link_domain.com www.link_domain.com;
 root /var/www/nama_folder_project/public;
 index index.php;
 location / {
 try_files $uri $uri/ /index.php?$query_string;
 }
 location ~ \.php$ {
 include snippets/fastcgi-php.conf;
 fastcgi_pass unix:/var/run/php/php8.2-fpm.sock; # Sesuaikan versi PHP jika berbeda
 }
}
```

## Konfigurasi nginx
```bash
sudo ln -s /etc/nginx/sites-available/master.conf /etc/nginx/sites-enabled/
```
```bash
sudo nginx -t
```
```bash
sudo systemctl reload nginx
```
## Setting SSL/https

```bash
sudo snap install core; sudo snap refresh core
```
```bash
sudo apt remove certbot
```
```bash
sudo snap install --classic certbot
```
```bash
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
```bash
sudo ufw status
```
```bash
sudo ufw enable
```
```bash
sudo ufw allow 'OpenSSH'
```
```bash
sudo ufw allow 'Nginx Full'
```
```bash
sudo ufw delete allow 'Nginx HTTP'
```
```bash
sudo ufw status
```
```bash
sudo certbot --nginx -d link_domain.com -d www.link_domain.com
```
```bash
sudo systemctl status snap.certbot.renew.service
```
```bash
sudo certbot renew --dry-run
```
```bash

```





