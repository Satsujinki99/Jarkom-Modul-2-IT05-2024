# Jarkom-Modul-2-IT05-2024

| Nama | NRP |
| ---- | :-: |
| Adlya Isriena Aftarisya | 5027231066 |
| Furqon Aryadana | 5027231024 |

## Daftar Isi
- [Soal 1](#soal-1)
- [Soal 2](#soal-2)
- [Soal 3](#soal-3)
- [Soal 4](#soal-4)
- [Soal 5](#soal-5)
- [Soal 6](#soal-6)
- [Soal 7](#soal-7)
- [Soal 8](#soal-8)
- [Soal 9](#soal-9)
- [Soal 10](#soal-10)
- [Soal 11](#soal-11)
- [Soal 12](#soal-12)
- [Soal 13](#soal-13)

# Soal 1
Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave. 
## Penyelesaian
### Sriwijaya (DNS Master)
lakukan instalasi bind9 pada ```nano /root/.bashrc```
```
apt-get update
apt-get install bind9 -y
```
### Majapahit (DNS Slave)
lakukan instalasi bind9 dan tambahkan IP Address DNS Master pada ```nano /root/.bashrc```
```
apt-get update
apt-get install bind9 -y
echo 'nameserver 10.66.1.3' > /etc/resolv.conf
```

# Soal 2
Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke Solok dengan alamat sudarsana.xxxx.com dengan alias www.sudarsana.xxxx.com, dimana xxxx merupakan kode kelompok. Contoh: sudarsana.it01.com.
## Penyelesaian
Edit /etc/resolv.conf pada setiap client dan tambahkan IP Sriwijaya (DNS Master) dengan ```nano /root/.bashrc```
```
echo 'nameserver 10.66.1.3' > /etc/resolv.conf
```
Lakukan setup domain pada Sriwijaya (DNS Master)
```
#!/bin/bash

# Buat domain sudarsana.it05.com
echo 'zone "sudarsana.it05.com" {
	type master;
	file "/etc/bind/jarkom/sudarsana.it05.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it05.com

# config domain
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it05.com. sudarsana.it05.com. (
                        2024100115      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it05.com.
@       IN      A       10.66.1.4     ; IP Solok
www     IN      CNAME   sudarsana.it05.com.' > /etc/bind/jarkom/sudarsana.it05.com

service bind9 restart
```

# Soal 3
Para pasukan juga perlu mengetahui mana titik yang akan diserang, sehingga dibutuhkan domain lain yaitu pasopati.xxxx.com dengan alias www.pasopati.xxxx.com yang mengarah ke Kotalingga.
## Penyelesaian
Edit sriwijaya2.bashrc dan tambahkan named.config.local untuk domain pasopati
```
#!/bin/bash

# Buat domain sudarsana.it05.com
echo 'zone "sudarsana.it05.com" {
	type master;
	file "/etc/bind/jarkom/sudarsana.it05.com";
};
zone "pasopati.it05.com" {
	type master;
	file "/etc/bind/jarkom/pasopati.it05.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it05.com

# config domain
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it05.com. sudarsana.it05.com. (
                        2024100115      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it05.com.
@       IN      A       10.66.1.4     ; IP Solok
www     IN      CNAME   sudarsana.it05.com.' > /etc/bind/jarkom/sudarsana.it05.com

service bind9 restart
```
Tambahkan config domain pasopati pada sriwijaya
```
cp /etc/bind/db.local /etc/bind/jarkom/pasopati.it05.com

# config domain
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it05.com. pasopati.it05.com. (
                        2024100116      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it05.com.
@       IN      A       10.66.2.3     ; IP Kotalingga
www     IN      CNAME   sudarsana.it05.com.' > /etc/bind/jarkom/pasopati.it05.com

service bind9 restart
```
## Testing
lakukan test pada client
```
ping pasopati.it05.com
```

# Soal 4
Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi dan suplai meme terbaru tersebut mengarah ke Tanjungkulai dan domain yang ingin digunakan adalah rujapala.xxxx.com dengan alias www.rujapala.xxxx.com.
## Penyelesaian
edit sriwijaya2.bashrc dan tambahkan named.conf.local untuk domain rujapala
```
#!/bin/bash

# Buat domain sudarsana.it05.com
echo 'zone "sudarsana.it05.com" {
	type master;
	file "/etc/bind/jarkom/sudarsana.it05.com";
};
zone "pasopati.it05.com" {
	type master;
	file "/etc/bind/jarkom/pasopati.it05.com";
};
zone "rujapala.it05.com" {
	type master;
	file "/etc/bind/jarkom/rujapala.it05.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it05.com

# config domain
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it05.com. root.sudarsana.it05.com. (
                        2024100115      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it05.com.
@       IN      A       10.66.1.4     ; IP Solok
www     IN      CNAME   sudarsana.it05.com.' > /etc/bind/jarkom/sudarsana.it05.com

service bind9 restart
```
Tambahkan config domain rujapala pada sriwijaya
```
cp /etc/bind/db.local /etc/bind/jarkom/rujapala.it05.com

# config domain
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it05.com. root.rujapala.it05.com. (
                        2024100117      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it05.com.
@       IN      A       10.66.2.4     ; IP Tanjungkulai
www     IN      CNAME   rujapala.it05.com.' > /etc/bind/jarkom/rujapala.it05.com

service bind9 restart
```
## Testing
lakukan test pada setiap client
```
ping rujapala.it05.com
```

# Soal 5
Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Nusantara.
## penyelesaian
pastikan /etc/resolv.conf pada setiap client terdapat IP Sriwijaya
```
nameserver 10.66.1.3
```

# Soal 6
Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain pasopati.xxxx.com melalui alamat IP Kotalingga (Notes: menggunakan pointer record).
## Penyelesaian
menggunakan reverse IP (PTR Record) dari IP Kotalingga (10.66.2.3) ke pasopati.it05.com
```
# edit sriwijaya2.bashrc

# Buat domain sudarsana.it05.com
echo 'zone "sudarsana.it05.com" {
	type master;
	file "/etc/bind/jarkom/sudarsana.it05.com";
};
zone "pasopati.it05.com" {
	type master;
	file "/etc/bind/jarkom/pasopati.it05.com";
};
zone "rujapala.it05.com" {
	type master;
	file "/etc/bind/jarkom/rujapala.it05.com";
};
zone "2.66.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.66.10.in-addr.arpa";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it05.com

# config domain
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it05.com. root.sudarsana.it05.com. (
                        2024100115      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it05.com.
@       IN      A       10.66.1.4     ; IP Solok
www     IN      CNAME   sudarsana.it05.com.' > /etc/bind/jarkom/sudarsana.it05.com

service bind9 restart
```
tambahkan script untuk config reverse domain pada sriwijaya
```
cp /etc/bind/db.local /etc/bind/jarkom/2.66.10.in-addr.arpa

echo ';
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it05.com. root.pasopati.it05.com. (
                        2024100121      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
2.66.10.in-addr.arpa.    IN      NS      pasopati.it05.com.
3                       IN      PTR     pasopati.it05.com.' > /etc/bind/jarkom/2.66.10.in-addr.arpa

service bind9 restart
```
lakukan setup config pada setiap client
```
# Install package dnsutils
apt-get update
apt-get install dnsutils -y

# Kembalikan nameserver ke IP Sriwijaya (DNS Master) & Majapahit (DNS slave)
echo '
nameserver 10.66.1.3
nameserver 10.66.2.2' > /etc/resolv.conf
```
## Testing
```
host -t PTR 10.66.2.3 #IP Kotalingga
```

# Soal 7
Akhir-akhir ini seringkali terjadi serangan brainrot ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Majapahit untuk semua domain yang sudah dibuat sebelumnya yang mengarah ke Sriwijaya (DNS Master)
## Penyelesaian
edit isi sriwijay2.bashrc dan tambahkan IP Majapahit (DNS Slave) 
```
#!/bin/bash
;
echo 'zone "sudarsana.it05.com" {
	type master;
	also-notify { 10.66.2.2; }; 
  allow-transfer { 10.66.2.2; };
	file "/etc/bind/jarkom/sudarsana.it05.com";
};
zone "pasopati.it05.com" {
	type master;
	also-notify { 10.66.2.2; };
  allow-transfer { 10.66.2.2; };
	file "/etc/bind/jarkom/pasopati.it05.com";
};
zone "rujapala.it05.com" {
	type master;
	also-notify { 10.66.2.2; };
  allow-transfer { 10.66.2.2; };
	file "/etc/bind/jarkom/rujapala.it05.com";
};
zone "2.66.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.66.10.in-addr.arpa";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it05.com

# config domain
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it05.com. root.sudarsana.it05.com. (
                        2024100115      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it05.com.
@       IN      A       10.66.1.4     ; IP Solok
www     IN      CNAME   sudarsana.it05.com.' > /etc/bind/jarkom/sudarsana.it05.com

service bind9 restart
```
tambahkan domain dan IP Sriwijaya (DNS Master) pada Majapahit (DNS Slave) 
```
echo 'zone "sudarsana.it05.com" {
	type slave;
	masters { 10.66.1.3; };
	file "/etc/bind/jarkom/sudarsana.it05.com";
};
zone "pasopati.it05.com" {
	type slave;
	masters { 10.66.1.3; };
	file "/etc/bind/jarkom/pasopati.it05.com";
};
zone "rujapala.it05.com" {
	type slave;
	masters { 10.66.1.3; };
	file "/etc/bind/jarkom/rujapala.it05.com";
};' > /etc/bind/named.conf.local

service bind9 restart
```
## Testing
stop bind9 pada sriwijaya
```
service bind9 stop
```
dan coba ping domain pada setiap client

# Soal 8
Kamu juga diperintahkan untuk membuat subdomain khusus melacak kekuatan tersembunyi di Ohio dengan subdomain cakra.sudarsana.xxxx.com yang mengarah ke Bedahulu.
## Penyelesaian 
modifikasi isi config domain sudarsana di sriwijaya2.bashrc
```
#!/bin/bash

# Buat domain sudarsana.it05.com
echo 'zone "sudarsana.it05.com" {
	type master;
	file "/etc/bind/jarkom/sudarsana.it05.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it05.com

# config domain
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it05.com. sudarsana.it05.com. (
                        2024100115      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it05.com.
@       IN      A       10.66.1.4     ; IP Solok
www     IN      CNAME   sudarsana.it05.com.
cakra   IN      A       10.66.2.5     ; IP Bendahulu' > /etc/bind/jarkom/sudarsana.it05.com

service bind9 restart
```
## Testing
lakukan test pada client
```
ping cakra.sudarsana.it05.com
```

# Soal 9
Karena terjadi serangan DDOS oleh shikanoko nokonoko koshitantan (NUN), sehingga sistem komunikasinya terhalang. Untuk melindungi warga, kita diperlukan untuk membuat sistem peringatan dari siren man oleh Frekuensi Freak dan memasukkannya ke subdomain panah.pasopati.xxxx.com dalam folder panah dan pastikan dapat diakses secara mudah dengan menambahkan alias www.panah.pasopati.xxxx.com dan mendelegasikan subdomain tersebut ke Majapahit dengan alamat IP menuju radar di Kotalingga
## Penyelesaian
Tambahkan script pada Sriwijaya (DNS Master), yaitu sriwijaya6.bashrc
```
#modifikasi isi file /etc/bind/named.conf.options
echo '
options {
        directory "/var/cache/bind";

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```
Tambahkan script di Majapahit, yaitu majahit2.bashrc
```
# membuat folder panah
mkdir /etc/bind/panah

#copy isi file pasopati.it05.com ke folder panah
cp /etc/bind/jarkom/pasopati.it05.com /etc/bind/panah/panah.pasopati.it05.com

#modifikasi isi file
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it05.com. panah.pasopati.it05.com. (
                        2024100116      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it05.com.
@       IN      A       10.66.2.3     ; IP Kotalingga
www     IN      CNAME   panah.pasopati.it05.com.
panah   IN      A       10.66.2.2     ; IP Majapahit
ns1     IN      A       10.66.2.2     ; IP Majapahit
panah   IN      NS      ns1
@       IN      AAAA    ::1' > /etc/bind/panah/panah.pasopati.it05.com

#modifikasi isi file /etc/bind/named.conf.options
echo '
options {
        directory "/var/cache/bind";

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```
Modifikasi majapahit.bashrc
```
echo 'zone "sudarsana.it05.com" {
	type slave;
	masters { 10.66.1.3; };
	file "/etc/bind/jarkom/sudarsana.it05.com";
};
zone "pasopati.it05.com" {
	type slave;
	masters { 10.66.1.3; };
	file "/etc/bind/jarkom/pasopati.it05.com";
};
zone "rujapala.it05.com" {
	type slave;
	masters { 10.66.1.3; };
	file "/etc/bind/jarkom/rujapala.it05.com";
};
zone "panah.pasopati.it05.com" {
    type master;
    allow-transfer { 10.66.1.3; };
    file "/etc/bind/panah/panah.pasopati.it05.com";
};' > /etc/bind/named.conf.local

service bind9 restart
```
## Testing
sebelumnya pastikan terlebih dahulu pada client bahwa nameserver mengarah pada IP majapahit
```
ping www.panah.pasopati.it05.com
```

# Soal 10
Markas juga meminta catatan kapan saja meme brain rot akan dijatuhkan, maka buatlah subdomain baru di subdomain panah yaitu log.panah.pasopati.xxxx.com serta aliasnya www.log.panah.pasopati.xxxx.com yang juga mengarah ke Kotalingga.
## Penyelesaian
modifikasi isi majapahit2.bashrc
```
# membuat folder panah
mkdir /etc/bind/panah

#copy isi file pasopati.it05.com ke folder panah
cp /etc/bind/jarkom/pasopati.it05.com /etc/bind/panah/panah.pasopati.it05.com

#modifikasi isi file
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it05.com. panah.pasopati.it05.com. (
                        2024100116      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it05.com.
@       IN      A       10.66.2.3     ; IP Kotalingga
www     IN      CNAME   panah.pasopati.it05.com.
log     IN      A       10.66.2.2     ; IP Majapahit
www.log IN      CNAME   panah.pasopati.it05.com.
@       IN      AAAA    ::1' > /etc/bind/panah/panah.pasopati.it05.com

#modifikasi isi file /etc/bind/named.conf.options
echo '
options {
        directory "/var/cache/bind";

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```
## Testing
pada client pastikan bahwa nameserver mengarah pada IP Majapahit
```
ping log.panah.pasopati.it05.com
ping www.log.panah.pasopati.it05.com
```

# Soal 11
Setelah pertempuran mereda, warga IT dapat kembali mengakses jaringan luar dan menikmati meme brainrot terbaru, tetapi hanya warga Majapahit saja yang dapat mengakses jaringan luar secara langsung. Buatlah konfigurasi agar warga IT yang berada diluar Majapahit dapat mengakses jaringan luar melalui DNS Server Majapahit
## Penyelesaian
modifikasi isi file ```/etc/bind/named.conf.options``` pada sriwijaya6.bashrc
```
#modifikasi isi file /etc/bind/named.conf.options
echo '
options {
        directory "/var/cache/bind";
        
        forwarders {
                192.168.122.1;
        };

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```
## Testing
lakukan test pada client
```
ping www.google.com
```

# Soal 12
Karena pusat ingin sebuah laman web yang ingin digunakan untuk memantau kondisi kota lainnya maka deploy laman web ini (cek resource yg lb) pada Kotalingga menggunakan apache
## Penyelesaian
tambahkan script berikut ke kotalingga.bashrc
```
if ! command -v named &> /dev/null
then
    echo "Apache2 belum terinstal, melakukan instalasi..."
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y
else
    echo "apache2 sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "Unzip belum terinstal, melakukan instalasi..."
    apt-get update
    apt-get install unzip -y
else
    echo "unzip sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "PHP belum terinstal, melakukan instalasi..."
    apt-get update
    apt-get install php -y
else
    echo "php sudah terinstal."
fi

# Download file lb.zip
curl -L -o lb.zip --insecure "https://drive.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7"

# Unzip file lb.zip
unzip lb.zip

# Hapus file template
rm -rf /var/www/html/index.php

# Copy file index.php
cp worker/index.php /var/www/html/index.php

service apache2 restart
```
Lakukan instalasi lynx pada client dengan script berikut:
```
# Cek apakah lynx sudah terinstal
if ! command -v named &> /dev/null
then
    echo "Lynx belum terinstal, melakukan instalasi..."
    # Melakukan instalasi lynx
    apt-get update
    apt-get install lynx -y
else
    echo "lynx sudah terinstal."
fi
```
## Testing
```
lynx http://10.66.2.3/index.php
```

# Soal 13
Karena Sriwijaya dan Majapahit memenangkan pertempuran ini dan memiliki banyak uang dari hasil penjarahan (sebanyak 35 juta, belum dipotong pajak) maka pusat meminta kita memasang load balancer untuk membagikan uangnya pada web nya, dengan Kotalingga, Bedahulu, Tanjungkulai sebagai worker dan Solok sebagai Load Balancer menggunakan apache sebagai web server nya dan load balancer nya
## Penyelesaian
Lakukan setup pada worker (Kotalingga, Bendahulu, Tangjungkulai) dengan script berikut
```

# install dependencies
if ! command -v named &> /dev/null
then
    echo "Apache2 belum terinstal, melakukan instalasi..."
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y
else
    echo "apache2 sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "Unzip belum terinstal, melakukan instalasi..."
    apt-get update
    apt-get install unzip -y
else
    echo "unzip sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "PHP belum terinstal, melakukan instalasi..."
    apt-get update
    apt-get install php -y
else
    echo "php sudah terinstal."
fi

# Download file lb.zip
curl -L -o lb.zip --insecure "https://drive.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7"

# Unzip file lb.zip
unzip lb.zip

# Hapus file template
rm -rf /var/www/html/index.php

# Copy file index.php
cp worker/index.php /var/www/html/index.php

service apache2 restart
```
setup Solok
```

if ! command -v named &> /dev/null
then
    echo "Apache2 belum terinstal, melakukan instalasi..."
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y
else
    echo "apache2 sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "PHP belum terinstal, melakukan instalasi..."
    apt-get update
    apt-get install php -y
else
    echo "php sudah terinstal."
fi

# Enable apache2 module
a2enmod proxy_balancer
a2enmod proxy_http
a2enmod lbmethod_byrequests
echo '
<VirtualHost *:80>
    <Proxy balancer://serverpool>
        BalancerMember http://10.75.2.2/
        BalancerMember http://10.75.2.3/
        BalancerMember http://10.75.2.4/
        Proxyset lbmethod=byrequests
    </Proxy>
    ProxyPass / balancer://serverpool/
    ProxyPassReverse / balancer://serverpool/
</VirtualHost>' > /etc/apache2/sites-available/000-default.conf

service apache2 restart
```
## Testing
lakukan testing pada client
```
lynx http://10.66.1.4/index.php
```
