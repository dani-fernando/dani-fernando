---
title: How to Install LAMP Server on Redhat
published: 2025-01-04
description: 'Guide installation LAMP server on Redhat, Almalinux, Fedora, Centos and Rockylinux'
image: './image/thumbnail-lamp-redhat.webp'
tags: [Redhat, Almalinux, Fedora, CentOS, Rocky Linux]
category: 'Linux'
draft: false 
lang: 'id'
---
### **🌟 Panduan Lengkap Install LAMP Server di Distro Turunan Fedora**  

Mau bikin server lokal untuk website atau aplikasi?   
Yuk, belajar install **LAMP Server** di distro turunan Fedora seperti **Redhat**, **AlmaLinux**, **Fedora**, **CentOS** dan **Rocky Linux** 🚀.  

### **Apa itu LAMP Server?**  
**LAMP** adalah singkatan dari:  
- **Linux**: Sistem operasi server.  
- **Apache**: Web server untuk meng-host website.  
- **MySQL/MariaDB**: Database untuk menyimpan data.  
- **PHP**: Bahasa pemrograman untuk aplikasi web.  

---

### **Step-by-Step Installasi LAMP Server**  

#### 🔥 **Step 1: Update Repository**  
Sebelum mulai, pastikan repository kamu up-to-date.  
```bash
sudo dnf update -y
```  

#### 🤖 **Step 2: Install Apache (Web Server)**  
Apache adalah software untuk meng-host website.  
```bash
sudo dnf install httpd -y
```  
**Start & Enable Apache:**  
```bash
sudo systemctl enable --now httpd
sudo systemctl status httpd
```  
**Add service http & https in firewall:**  
```bash
sudo firewall-cmd --zone=public --add-service=http --permanent
sudo firewall-cmd --zone=public --add-service=https --permanent
sudo firewall-cmd --reload
```  

**Chek Apache Page:**  
Buka browser dan akses `http://localhost` atau IP server kamu. Maka akan muncul **default page apache**.
![Apache redhat Default Page](https://www.redhat.com/rhdc/managed-files/sysadmin/2019-09/LAMP-Apache_Test.png)
> Setiap distro memiliki default page apache yang berbeda - beda.

---
#### 💾 **Step 3: Install MySQL (Database)**  
Database digunakan untuk menyimpan data website.  
```bash
sudo dnf install mysql mysql-server -y
```  
**Start & Enable MySQL Server**  
```bash
sudo systemctl enable --now mysqld
sudo systemctl status mysqld
```  
**MySQL Secure Installation**  
**1. Run script**
```bash
sudo mysql_secure_installation
```
**2. Press yes**
```bash
Would you like to setup VALIDATE PASSWORD component? (Press y|Y for Yes, any other key for No): yes
```  
**3. Select Password Security Level**
```bash
There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters, and dictionary file

Please enter 0 = LOW, 1 = MEDIUM, 2 = STRONG: 
```  

| LEVEL  | PENJELASAN                                                                |
|--------|---------------------------------------------------------------------------|
| LOW    | Hanya membutuhkan panjang minimum 8 karakter.                             |
| MEDIUM | Kombinasi angka, huruf besar, huruf kecil, dan karakter khusus.           |
| STRONG | Seperti MEDIUM, tetapi dengan tambahan validasi terhadap kata dari kamus. |

> *Pilih level password sesuai kebutuhanmu*  

**4. Set root password**
```bash
Please set the password for root here. New password:
Re-enter new password:
```  
**5. Remove anonymous user**
```bash
Remove anonymous users? (Press y|Y for Yes, any other key for No):yes
```  
| OPSI | PENJELASAN                                                                                                  |
|------|-------------------------------------------------------------------------------------------------------------|
| yes  | Menghapus akun anonymous yang memungkinkan siapa pun masuk tanpa akun pengguna. (Ini meningkatkan keamanan) |
| no   | Membiarkan akun anonymous tetap ada. (tidak disarankan)                                                     |

**6. Disallow root login remote**
```bash
Disallow root login remotely? (Press y|Y for Yes, any other key for No):yes
```  
| OPSI | PENJELASAN                                                                                                    |
|------|---------------------------------------------------------------------------------------------------------------|
| yes  | Mencegah pengguna root login dari jarak jauh melalui koneksi TCP. (Ini mengurangi risiko serangan jarak jauh) |
| no   | Membiarkan login jarak jauh untuk root. (tidak disarankan kecuali diperlukan untuk konfigurasi khusus)        |

**7. Remove Test Database**
```bash
Remove test database and access to it? (Press y|Y for Yes, any other key for No):yes
```  
| OPSI | PENJELASAN                                                                                                    |
|------|---------------------------------------------------------------------------------------------------------------|
| yes  | Menghapus database test yang dibuat secara default selama instalasi. (Database ini tidak digunakan dan dapat menjadi potensi celah keamanan) |
| no   | Membiarkan database test. (tidak disarankan)|

**8. Reload privilege tables**
```bash
Reload privilege tables now? (Press y|Y for Yes, any other key for No):yes
```
| OPSI | PENJELASAN                                                                    |
|------|-------------------------------------------------------------------------------|
| yes  | Memuat ulang tabel privilege untuk memastikan semua perubahan segera berlaku. |
| no   | Menunda pemuatan ulang hingga dilakukan secara manual.                        |

**9. Exit MySQL**
```
quit;
```  
---  
#### 👨‍💻 **Step 4: Install PHP (Scripting Language)**  
PHP digunakan untuk menjalankan aplikasi web dinamis.  
```bash
sudo dnf install php php-mysqlnd php-cli -y
```  
**Restart Apache untuk memuat PHP:**  
```bash
sudo systemctl restart httpd
```  
**Tes PHP:**  
Buat file PHP di `/var/www/html/`:  
```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```  
Akses `http://localhost/info.php` di browser. Kalau muncul informasi PHP, instalasi sukses!  
![phpinfo](https://www.redhat.com/rhdc/managed-files/sysadmin/2019-09/LAMP_PHPtest.png)

#### 🌐 **Step 5: Install phpMyAdmin (Opsional)**  
phpMyAdmin adalah tool berbasis web untuk mengelola database.  
```bash
sudo dnf install phpmyadmin php php-mbstring php-zip php-gd php-json php-curl php-mysqlnd -y
```  
**Edit Config Apache:**  
Tambahkan ini di `/etc/httpd/conf.d/phpMyAdmin.conf`:  
```apache
<Directory /usr/share/phpMyAdmin/>
    Require all granted
</Directory>
```  
**Restart Apache Lagi:**  
```bash
sudo systemctl restart httpd
```  

Akses di `http://localhost/phpmyadmin`   
![phpmyadmin-login](https://www.tecmint.com/wp-content/uploads/2016/09/PHPMyAdmin-Login.webp)

### **Kesimpulan:**  
Setelah mengikuti langkah-langkah ini, kamu punya server lokal yang powerful dan siap digunakan. ✨  

**Keuntungan LAMP Server:**  
1. 💻 **Open Source**: Gratis, fleksibel, dan komunitasnya besar.  
2. 🌍 **Universal**: Bisa dipakai untuk banyak jenis aplikasi web.  
3. 🚀 **Efisien**: Cocok untuk development atau server live.  

:::note
Jangan lupa untuk selalu update server dan backup data kamu secara rutin.  
Kalau kamu merasa postingan ini bermanfaat, share ke teman-temanmu! 
:::
 
**Selamat mencoba! Let’s build something awesome! 🚀**
![kesimpulan-lamp-redhat](./image/kesimpulan-lamp-redhat.jpg)