# Jarkom-Modul-2-IT21-2024

|Nama  | NRP |
|--|--|
| Nathan Kho Pancras | 5027221002 |
| Muhammad Andrean Rizq Prasetio | 5027221052 |

**Deskripsi** - Sebuah kerajaan besar di Indonesia sedang mengalami pertempuran dengan penjajah. Kerajaan tersebut adalah Sriwijaya. Karena merasa terdesak Sriwijaya meminta bantuan pada Majapahit untuk mempertahankan wilayahnya. Pertempuran besar tersebut berada di Nusantara. Untuk topologi lihat pada link ini.

## Prerequisites

### Topologi

![alt text](assets/topologi.png)

### Konfigurasi

```bash
vi /etc/network/interfaces
cat /etc/network/interfaces
```

**Nusantara**

```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.74.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.74.2.1
	netmask 255.255.255.0
```

**Majapahit**

```
auto eth0
iface eth0 inet static
	address 10.74.1.2
	netmask 255.255.255.0
	gateway 10.74.1.1
```

**Mulawarman**

```
auto eth0
iface eth0 inet static
	address 10.74.1.3
	netmask 255.255.255.0
	gateway 10.74.1.1
```


**GrahamBell**

```
auto eth0
iface eth0 inet static
	address 10.74.1.4
	netmask 255.255.255.0
	gateway 10.74.1.1
```

**Samaratungga**

```
auto eth0
iface eth0 inet static
	address 10.74.1.5
	netmask 255.255.255.0
	gateway 10.74.1.1
```

**Solok**

```
auto eth0
iface eth0 inet static
	address 10.74.2.2
	netmask 255.255.255.0
	gateway 10.74.2.1
```

**Srikandi**

```
auto eth0
iface eth0 inet static
	address 10.74.2.3
	netmask 255.255.255.0
	gateway 10.74.2.1
```

**Kotalingga**

```
auto eth0
iface eth0 inet static
	address 10.74.2.4
	netmask 255.255.255.0
	gateway 10.74.2.1
```

**Sriwijaya**

```
auto eth0
iface eth0 inet static
	address 10.74.2.5
	netmask 255.255.255.0
	gateway 10.74.2.1
```

**Tanjungkulai**

```
auto eth0
iface eth0 inet static
	address 10.74.2.6
	netmask 255.255.255.0
	gateway 10.74.2.1
```

**Bedahulu**

```
auto eth0
iface eth0 inet static
	address 10.74.2.7
	netmask 255.255.255.0
	gateway 10.74.2.1
```

### Set .bashrc

**Router (Nusantara)**

```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.74.0.0/16
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

**DNS Master**

Sriwijaya

```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
service bind9 start
```

**DNS Slave**

Majapahit, Tanjungkulai, Bedahulu

```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
service bind9 start
apt-get install apache2 -y
service apache2 start
apt-get install lynx -y
apt-get install php -y
```

**Clients**

```
sek nanti
sek nanti
sek nanti
sek nanti
sek nanti
sek nanti
```

## Soal

### No 1

Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave. 

**Pengerjaan**

Karena di `.bashrc` sudah melakukan instalasi ke package bind9, dan tambahan lain untuk DNS Slave (apache, lynx, php), nomor ini sudah selesai.

### No 2

Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke Solok dengan alamat sudarsana.xxxx.com dengan alias www.sudarsana.xxxx.com, dimana xxxx merupakan kode kelompok. Contoh: sudarsana.it01.com.

**Pengerjaan**

Sriwijaya - `sudarsana.sh`

```bash
echo 'zone "sudarsana.it21.com" {
	type master;
	file "/etc/bind/sudarsana/sudarsana.it21.com";
	};' >> /etc/bind/named.conf.local

mkdir /etc/bind/sudarsana
cp /etc/bind/db.local /etc/bind/sudarsana/sudarsana.it21.com
service bind9 restart

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it21.com. root.sudarsana.it21.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it21.com.
@       IN      A       10.74.2.2		; IP solok
www		IN		CNAME	sudarsana.it21.com.
@       IN      AAAA    ::1' >  /etc/bind/sudarsana/sudarsana.it21.com
```

### No 3

Untuk memastikan keamanan dan keandalan sistem, buatlah backup DNS server di Bedahulu yang akan berfungsi sebagai secondary DNS server. DNS server ini harus selalu sinkron dengan DNS Master di Sriwijaya.

Sriwijaya - `pasopati.sh`

```bash
echo 'zone "pasopati.it21.com" {
 	type master; >> /etc/bind/named.conf.local
 	file "/etc/bind/pasopati/pasopati.it21.com"; 
};' >> /etc/bind/named.conf.local

mkdir /etc/bind/pasopati

cp /etc/bind/db.local /etc/bind/pasopati/pasopati.it21.com

service bind9 restart

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it21.com. root.pasopati.it21.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it21.com.
@       IN      A       10.74.2.4		; IP kotalingga
www		IN		CNAME	pasopati.it21.com.
@       IN      AAAA    ::1' >  /etc/bind/pasopati/pasopati.it21.com
```

**Pengerjaan**

### No 4

Buatlah subdomain baru untuk keperluan komunikasi rahasia antara pasukan dengan nama rahasia.sudarsana.xxxx.com yang mengarah ke Bedahulu. Pastikan subdomain ini hanya dapat diakses oleh IP tertentu yang telah ditentukan oleh markas pusat.

Sriwijaya - `rujapala.sh`

```bash
echo 'zone "rujapala.it21.com" {
 	type master; >> /etc/bind/named.conf.local
 	file "/etc/bind/rujapala/rujapala.it21.com"; 
};' >> /etc/bind/named.conf.local

mkdir /etc/bind/rujapala

cp /etc/bind/db.local /etc/bind/rujapala/rujapala.it21.com

service bind9 restart

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it21.com. root.rujapala.it21.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it21.com.
@       IN      A       10.74.2.6		; IP tanjungkulai
www		IN		CNAME	rujapala.it21.com.
@       IN      AAAA    ::1' >  /etc/bind/rujapala/rujapala.it21.com
```

**Pengerjaan**

### No 5

Karena adanya ancaman dari pihak luar, buatlah firewall di setiap web server (Tanjungkulai, Bedahulu, dan Kotalingga) untuk memblokir akses dari IP yang mencurigakan. Pastikan firewall ini dapat diatur dan diupdate secara otomatis dari pusat komando di Sriwijaya.

**Pengerjaan**

### No 6

Untuk meningkatkan kecepatan akses, buatlah CDN (Content Delivery Network) dengan server di Tanjungkulai, Bedahulu, dan Kotalingga. Pastikan CDN ini dapat mendistribusikan konten secara efisien ke seluruh Nusantara.

**Pengerjaan**

### No 7

Buatlah sistem monitoring untuk memantau kinerja dan keamanan dari semua server (Tanjungkulai, Bedahulu, Kotalingga, dan Solok). Sistem monitoring ini harus dapat memberikan laporan secara real-time ke pusat komando di Sriwijaya.

**Pengerjaan**

### No 8

Untuk meningkatkan keamanan, implementasikan sistem autentikasi dua faktor (2FA) untuk semua akses ke server dan domain yang telah dibuat. Pastikan sistem ini dapat diintegrasikan dengan DNS Master di Sriwijaya.

**Pengerjaan**

### No 9

Buatlah sistem logging yang terpusat di Sriwijaya untuk mencatat semua aktivitas yang terjadi di semua server dan domain. Pastikan log ini dapat diakses dan dianalisis oleh tim keamanan di Sriwijaya.

**Pengerjaan**

### No 10

Untuk memastikan ketersediaan layanan, buatlah sistem failover yang akan mengalihkan trafik ke server cadangan jika server utama mengalami gangguan. Pastikan sistem ini dapat bekerja secara otomatis dan tanpa intervensi manual.

**Pengerjaan**

### No 11

Buatlah sistem load balancing yang lebih canggih dengan menggunakan algoritma round-robin dan least connections. Pastikan sistem ini dapat diatur dan dioptimalkan dari pusat komando di Sriwijaya.

**Pengerjaan**

### No 12

Implementasikan sistem enkripsi untuk semua komunikasi antara server dan client. Pastikan semua data yang dikirim dan diterima terenkripsi dengan baik untuk mencegah penyadapan.

**Pengerjaan**

### No 13

Buatlah sistem backup dan restore yang terpusat di Sriwijaya untuk semua data dan konfigurasi server. Pastikan sistem ini dapat melakukan backup secara otomatis dan restore dengan cepat jika terjadi kegagalan.

**Pengerjaan**

### No 14

Untuk meningkatkan performa, optimalkan konfigurasi web server dan database di semua server (Tanjungkulai, Bedahulu, Kotalingga, dan Solok). Pastikan semua server dapat menangani beban trafik yang tinggi dengan efisien.

**Pengerjaan**

### No 15

Buatlah sistem alert yang akan memberikan notifikasi ke tim IT jika terjadi masalah atau serangan ke server. Pastikan notifikasi ini dapat dikirim melalui email, SMS, dan aplikasi pesan instan.

**Pengerjaan**

### No 16

Implementasikan sistem caching di semua server untuk mengurangi beban dan meningkatkan kecepatan akses. Pastikan sistem caching ini dapat diatur dan dioptimalkan dari pusat komando di Sriwijaya.

**Pengerjaan**

### No 17

Buatlah sistem audit untuk memeriksa dan memastikan semua konfigurasi dan kebijakan keamanan telah diterapkan dengan benar di semua server dan domain.

**Pengerjaan**

### No 18

Untuk meningkatkan kolaborasi, buatlah sistem berbagi file yang aman dan terenkripsi di antara semua pasukan. Pastikan sistem ini dapat diakses dari semua server dan domain yang telah dibuat.

**Pengerjaan**

### No 19

Buatlah sistem pemulihan bencana (disaster recovery) yang terpusat di Sriwijaya. Pastikan sistem ini dapat memulihkan semua layanan dan data dengan cepat jika terjadi bencana atau kegagalan besar.

**Pengerjaan**

### No 20

Untuk memastikan semua sistem berjalan dengan baik, lakukan uji coba dan simulasi serangan secara berkala. Pastikan semua tim IT siap dan dapat merespon dengan cepat jika terjadi serangan atau masalah.

**Pengerjaan**