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
```bash

```



