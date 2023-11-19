# Jarkom-Modul-3-A05-2023

## Author

| Nama                 | NRP        |
| -------------------- | ---------- |
| Muhammad Febriansyah | 5025211164 |
| Layyinatul Fuadah    | 5025211207 |

## Topologi

![alt text](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/blob/main/img/topology.png?raw=true)

## Config

dilakukan konfigurasi sesuai dengan peta yang sudah diberikan yang dilakukan pada masing masing node. dengan prefix ip yaitu 192.171

- **Aura (DHCP Relay)**

```
auto eth0
iface eth0 inet dhcp
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.171.0.0/16

auto eth1
iface eth1 inet static
	address 192.171.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.171.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.171.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.171.4.0
	netmask 255.255.255.0

```

- **Himmel (DHCP Server)**

```
auto eth0
iface eth0 inet static
	address 192.171.1.1
	netmask 255.255.255.0
	gateway 192.171.1.0

```

- **Heiter (DNS Server)**

```
auto eth0
iface eth0 inet static
	address 192.171.1.2
	netmask 255.255.255.0
	gateway 192.171.1.0
```

- **Denken (Database Server)**

```
auto eth0
iface eth0 inet static
	address 192.171.2.1
	netmask 255.255.255.0
	gateway 192.171.2.0
```

- **Eisen (Load Balancer)**

```
auto eth0
iface eth0 inet static
	address 192.171.2.2
	netmask 255.255.255.0
	gateway 192.171.2.0
```

- **Lugner (PHP Worker)**

```

auto eth0
iface eth0 inet static
	address 192.171.3.1
	netmask 255.255.255.0
	gateway 192.171.3.0
```

- **Linie (PHP Worker)**

```
auto eth0
iface eth0 inet static
	address 192.171.3.2
	netmask 255.255.255.0
	gateway 192.171.3.0

```

- **Lawine (PHP Worker)**

```
auto eth0
iface eth0 inet static
	address 192.171.3.3
	netmask 255.255.255.0
	gateway 192.171.3.0

```

- **Fern (Laravel Worker)**

```

auto eth0
iface eth0 inet static
	address 192.171.4.1
	netmask 255.255.255.0
	gateway 192.171.4.0

```

- **Flamme (Laravel Worker)**

```
auto eth0
iface eth0 inet static
	address 192.171.4.2
	netmask 255.255.255.0
	gateway 192.171.4.0

```

- **Frieren (Laravel Worker)**

```
auto eth0
iface eth0 inet static
	address 192.171.4.3
	netmask 255.255.255.0
	gateway 192.171.4.0
```

- **Stark (Client Dynamic)**

  ```
  auto eth0
  iface eth0 inet dhcp

  ```

- **Sein (Client Dynamic)**

  ```
  auto eth0
  iface eth0 inet dhcp

  ```

- **Revolte (Client Dynamic)**

  ```
  auto eth0
  iface eth0 inet dhcp

  ```

- **Richter (Client Dynamic)**

  ```
  auto eth0
  iface eth0 inet dhcp

  ```

### Alamat Ip untuk Setiap Node

- **Alamat IP Pada Masing-masing Node**

  ```
  Aura (Router DHCP Relay)    : 192.171.1.0
  Himmel (DHCP Server)        : 192.171.1.1
  Heiter (DNS Serve)          : 192.171.1.2
  Denken (Database Server)    : 192.171.2.1
  Eisen (Load Balancer)       : 192.171.2.2
  Fern (Laravel Worker)	      : 192.171.4.1
  Flamme (Laravel Worker)     : 192.171.4.2
  Freiren (Laravel Worker)	  : 192.171.4.3
  Lugner (PHP Worker)	      : 192.171.3.1
  Linie (PHP Worker)          : 192.171.3.2
  Lawine (PHP Worker)	      : 192.171.3.3
  Richter (Client Dynamic)	  : 192.171.3.4
  Revolte (Client Dynamic)	  : 192.171.3.5
  Stark (Client Dynamic)	  : 192.171.4.4
  Sein (Client Dynamic)	      : 192.171.4.5

  ```

### Command Setup

dilakukan setup pada masing masing node yang disimpan pada file /root/.bashrc agar dapat dijalankan secara otomatis ketika node dibukak pada gns3

- Aura

  ```sh
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.171.0.0/16
  echo 'nameserver 192.168.122.1' > /etc/resolv.conf
  apt-get update
  apt install isc-dhcp-relay -y
  ```

  - DHCP Server

  ```sh
  echo 'nameserver 192.171.1.2' > /etc/resolv.conf
  apt-get update
  apt install isc-dhcp-server -y
  ```

- DNS Server

  ```sh
  echo 'nameserver 192.168.122.1' > /etc/resolv.conf
  apt-get update
  apt-get install bind9 -y
  ```

- Database Server

  ```sh
  echo 'nameserver 192.171.1.2' > /etc/resolv.conf
  apt-get update
  apt-get install mariadb-server -y
  service mysql start
  ```

  Lalu jangan lupa untuk mengganti [bind-address] pada file /etc/mysql/mariadb.conf.d/50-server.cnf menjadi 0.0.0.0 dan jangan lupa untuk melakukan restart mysql kembali

- Load Balancer

  ```sh
  echo 'nameserver 192.171.1.2' > /etc/resolv.conf
  apt-get update
  apt-get install apache2-utils -y
  apt-get install nginx -y
  apt-get install lynx -y

  service nginx start
  ```

- Laravel Worker

  ```sh
  echo 'nameserver 192.171.1.2' > /etc/resolv.conf
  apt-get update
  apt-get install lynx -y
  apt-get install mariadb-client -y
  # Test connection from worker to database
  # mariadb --host=192.171.2.1 --port=3306   --user=kelompoka05 --password=passworda05 dbkelompoka05 -e "SHOW DATABASES;"
  apt-get install -y lsb-release ca-certificates a   apt-transport-https software-properties-common gnupg2
  curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
  sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
  apt-get update
  apt-get install php8.0-mbstring php8.0-xml php8.0-cli   php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
  apt-get install nginx -y

  service nginx start
  service php8.0-fpm start
  ```

- PHP Worker

  ```sh
  echo 'nameserver 192.171.1.2' > /etc/resolv.conf
  apt-get update
  apt-get install nginx -y
  apt-get install wget -y
  apt-get install unzip -y
  apt-get install lynx -y
  apt-get install htop -y
  apt-get install apache2-utils -y
  apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-cli php7.3-zip -y

  service nginx start
  service php7.3-fpm start
  ```

- Client
  ```sh
  apt update
  apt install lynx -y
  apt install htop -y
  apt install apache2-utils -y
  apt-get install jq -y
  ```

## Soal 1

> Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

Pertama buka pada node heiter sebagai DNS Server kemudian dibuat domain bernama `riegel.canyon.a05.com` dan granz.channel.a05.com yang nantinya akan digunakan pada praktikum ini, untuk script dilakukan setup terlebih dahulu seperti diatas kemudian dilakukan konfigurasi sebagai berikut

### konfig

```sh
echo 'options {
      directory "/var/cache/bind";
      forwarders{
              192.168.122.1;
      };
      allow-query{any;};
      auth-nxdomain no;    # conform to RFC1035
      listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

echo 'zone "riegel.canyon.a05.com"{
      type master;
      file "/etc/bind/jarkom_a05/riegel.canyon.a05.com";
};' > /etc/bind/named.conf.local
echo 'zone "granz.channel.a05.com"{
      type master;
      file "/etc/bind/jarkom_a05/granz.channel.a05.com";
};' >> /etc/bind/named.conf.local


mkdir /etc/bind/jarkom_a05
cp /etc/bind/db.local /etc/bind/jarkom_a05/riegel.canyon.a05.com
cp /etc/bind/db.local /etc/bind/jarkom_a05/granz.channel.a05.com
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.a05.com. root.riegel.canyon.a05.com. (
                            2         ; Serial
                       604800         ; Refresh
                        86400         ; Retry
                      2419200         ; Expire
                       604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.a05.com.
@       IN      A       192.171.2.2       ; IP
www     IN      CNAME   riegel.canyon.a05.com.
@       IN      AAAA    ::1' > /etc/bind/jarkom_a05/riegel.canyon.a05.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.www.com. root.granz.channel.a05.com. (
                            2         ; Serial
                       604800         ; Refresh
                        86400         ; Retry
                      2419200         ; Expire
                       604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.a05.com.
@       IN      A       192.171.2.2       ; IP
www     IN      CNAME   granz.channel.a05.com.
@       IN      AAAA    ::1' > /etc/bind/jarkom_a05/granz.channel.a05.com

service bind9 start


echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt install nginx php php-fpm -y

```

### output

kemudian dilakukan pengecekan apakah sudah terkoneksi dengan benar serta dapat terhubung dengan domain yang dimasukkan pada node himmel dan node lamine
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

## Soal 2

Semua CLIENT harus menggunakan konfigurasi dari DHCP Server. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80

dilakukan setup terlebih dahulu kepada node yang ingin digunakan seperti diatas kemudian pada node himmel dilakukan konfigurasi sebagai berikut untuk menjawab soal nomor 2

### Script

```sh
echo 'subnet 192.171.1.0 netmask 255.255.255.0 {
}

subnet 192.171.2.0 netmask 255.255.255.0 {
}

subnet 192.171.3.0 netmask 255.255.255.0 {
    range 192.171.3.16 192.171.3.32; #soal 2
    range 192.171.3.64 192.171.3.80; #soal 2
    option routers 192.171.3.0; #soal 2
    option broadcast-address 192.171.3.255; #soal 4
    option domain-name-servers 192.171.1.2; #soal 4
    default-lease-time 180; #soal 5
    max-lease-time 5760; #soal 5
} #switch 3

subnet 192.171.4.0 netmask 255.255.255.0 {
    range 192.171.4.12 192.171.4.20; #soal 3
    range 192.171.4.160 192.171.4.168; #soal 3
    option routers 192.171.4.0; #soal 3
    option broadcast-address 192.171.4.255; #soal 4
    option domain-name-servers 192.171.1.2; #soal 4
    default-lease-time 720; #soal 5
    max-lease-time 5760; #soal 5
} #switch 4' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

### output

![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

## Soal 3

> Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168

dilakukan setup terlebih dahulu kepada node yang ingin digunakan seperti diatas kemudian pada node himmel dilakukan konfigurasi sebagai berikut untuk menjawab soal nomor 3

### Script

```sh
echo 'subnet 192.171.1.0 netmask 255.255.255.0 {
}

subnet 192.171.2.0 netmask 255.255.255.0 {
}

subnet 192.171.3.0 netmask 255.255.255.0 {
    range 192.171.3.16 192.171.3.32; #soal 2
    range 192.171.3.64 192.171.3.80; #soal 2
    option routers 192.171.3.0; #soal 2
    option broadcast-address 192.171.3.255; #soal 4
    option domain-name-servers 192.171.1.2; #soal 4
    default-lease-time 180; #soal 5
    max-lease-time 5760; #soal 5
} #switch 3

subnet 192.171.4.0 netmask 255.255.255.0 {
    range 192.171.4.12 192.171.4.20; #soal 3
    range 192.171.4.160 192.171.4.168; #soal 3
    option routers 192.171.4.0; #soal 3
    option broadcast-address 192.171.4.255; #soal 4
    option domain-name-servers 192.171.1.2; #soal 4
    default-lease-time 720; #soal 5
    max-lease-time 5760; #soal 5
} #switch 4' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

### Output

![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

## Soal 4

> Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

dilakukan setup terlebih dahulu kepada node yang ingin digunakan seperti diatas kemudian pada node himmel dilakukan konfigurasi sebagai berikut untuk menjawab soal nomor 4 dengan menambahkan beberapa konfigurasi seperti `option broadcast-address` dan `option domain-name-server` agar dapat DNS yang telah disiapkan sebelumnya dapat digunakan

### Script

```sh
echo 'subnet 192.171.1.0 netmask 255.255.255.0 {
}

subnet 192.171.2.0 netmask 255.255.255.0 {
}

subnet 192.171.3.0 netmask 255.255.255.0 {
    range 192.171.3.16 192.171.3.32; #soal 2
    range 192.171.3.64 192.171.3.80; #soal 2
    option routers 192.171.3.0; #soal 2
    option broadcast-address 192.171.3.255; #soal 4
    option domain-name-servers 192.171.1.2; #soal 4
    default-lease-time 180; #soal 5
    max-lease-time 5760; #soal 5
} #switch 3

subnet 192.171.4.0 netmask 255.255.255.0 {
    range 192.171.4.12 192.171.4.20; #soal 3
    range 192.171.4.160 192.171.4.168; #soal 3
    option routers 192.171.4.0; #soal 3
    option broadcast-address 192.171.4.255; #soal 4
    option domain-name-servers 192.171.1.2; #soal 4
    default-lease-time 720; #soal 5
    max-lease-time 5760; #soal 5
} #switch 4' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

### Output

![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

## Soal 5

> Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit

dilakukan setup terlebih dahulu kepada node yang ingin digunakan seperti diatas kemudian pada node himmel dilakukan konfigurasi sebagai berikut untuk menjawab soal nomor 5 dengan menambahkan fungsi default-lease-time dan max-lease-time pada masing" switch sesuai dengan ketentuan

### Script

```sh
echo 'subnet 192.171.1.0 netmask 255.255.255.0 {
}

subnet 192.171.2.0 netmask 255.255.255.0 {
}

subnet 192.171.3.0 netmask 255.255.255.0 {
    range 192.171.3.16 192.171.3.32; #soal 2
    range 192.171.3.64 192.171.3.80; #soal 2
    option routers 192.171.3.0; #soal 2
    option broadcast-address 192.171.3.255; #soal 4
    option domain-name-servers 192.171.1.2; #soal 4
    default-lease-time 180; #soal 5
    max-lease-time 5760; #soal 5
} #switch 3

subnet 192.171.4.0 netmask 255.255.255.0 {
    range 192.171.4.12 192.171.4.20; #soal 3
    range 192.171.4.160 192.171.4.168; #soal 3
    option routers 192.171.4.0; #soal 3
    option broadcast-address 192.171.4.255; #soal 4
    option domain-name-servers 192.171.1.2; #soal 4
    default-lease-time 720; #soal 5
    max-lease-time 5760; #soal 5
} #switch 4' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
```

### Output

![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

## Soal 6

> Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. (6)

dilakukan setup terlebih dahulu pada masing masing worker PHP yaitu lawine, Linie, dan Lugner seperti yang diatas. kemudian pada masing-masing worker pada kasus ini worker lawine dilakukan dowload dari file drive lalu di unzip kemudian dilakukan konfigurasi pada '''nginx''' seperti berikut

### Script

```sh
mv modul-3 /var/www/granz.channel.a05.com
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/granz.channel.a05.com
ln -s /etc/nginx/sites-available/granz.channel.a05.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo 'server {
    listen 80;
    server_name _;

    root /var/www/granz.channel.a05.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock;  # Sesuaikan versi PHP dan socket
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/granz.channel.a05.com

service nginx restart
```

### Output

jalankan perintah '''lynx localhost''' pada node client

![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

## Soal 7

> Kepala suku dari Bredt Region memberikan resource server sebagai berikut: Lawine, 4GB, 2vCPU, dan 80 GB SSD. Linie, 2GB, 2vCPU, dan 50 GB SSD. Lugner 1GB, 1vCPU, dan 25 GB SSD. Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)

Sebelum mengerjakan perlu untuk melakukan [setup](#sebelum-memulai) terlebih dahulu. Setelah melakukan konfigurasi diatas, sekarang lakukan konfigurasi `Load Balancing` pada node `Eisen` sebagai berikut
dilakukan setup terlebih dahulu pada node '''eisen''' seperti yagn dijelaskan diatas, kemudian dilakukan konfigurasi '''load balancing''' dengan langkah-langkah sebagai berikut

### Script

mula mula dilakukan konfigurasi pada node Heiter (DNS Server), seperti pada gambar untuk kedua domain '''granz.channel.a05.com''' dan '''riegel.canyon.a05.com''' dengan mengarahkan IP menuju node Eisen
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

kemudian buka pada node eisen lalu lakukan konfigurasi sebagai berikut

```sh
mv modul-3 /var/www/granz.channel.a05.com

echo '
server {

listen 80;

root /var/www/granz.channel.a05.com;

index index.php index.html index.htm;
server_name _;

location / {
        try_files $uri $uri/ /index.php?$query_string;
}

# pass PHP scripts to FastCGI server
location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
}

location ~ /\.ht {
        deny all;
}

error_log /var/log/nginx/granz.channel.a05.com_error.log;
access_log /var/log/nginx/granz.channel.a05.com_access.log;
}
' > /etc/nginx/sites-available/granz.channel.a05.com

ln -s /etc/nginx/sites-available/granz.channel.a05.com /etc/nginx/sites-enabled
rm /etc/nginx/sites-enabled/default
service php7.2-fpm start
service nginx restart
```

### Output

pada node client kali ini saya menggunakan node sein yang telah dikonfigurasi dengan perintah dibawah

```sh
ab -n 1000 -c 100 http://192.171.2.2/
```

yang diperoleh hasil sebagai berikut

![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

## Soal 10

> Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

mula" dilakukan setup untuk node Eisen sebagai load balance dengan username netics dan pasword ajka05, dengan membuat folder baru bernama '''rahasisakita''' lalu user dan pw disimpan disana dengan cara menambahkan '''auth_basic "Restricted Content";
auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;''' pada nginx di '''location \ '''

### Script

```sh
mkdir /etc/nginx/rahasisakita
htpasswd -b -c /etc/nginx/rahasisakita/htpasswd netics ajka05
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/konfiq

echo 'upstream worker {
    server 192.171.3.1;
    server 192.171.3.2;
    server 192.171.3.3;
}

server {
    listen 80;
    server_name granz.channel.a05.com www.granz.channel.a05.com;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
	allow 192.171.3.69; #soal 12
        allow 192.171.3.70; #soal12
        allow 192.171.4.167; #soal12
        allow 192.171.4.168; #soal12
        deny all;
        proxy_pass http://worker; #soal8
	auth_basic "Restricted Content"; #soal10
	auth_basic_user_file /etc/nginx/rahasisakita/htpasswd; #soal10
    }

    location ~ /its { #soal11
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}' > /etc/nginx/sites-available/konfiq

ln -s /etc/nginx/sites-available/konfiq /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

kemudian ketik '''htpasswd -c /etc/nginx/rahasisakita/htpasswd netics''', Lalu, masukkan passwordnya `ajka05`

### Output

kemudian akses url `http://www.granz.channel.a05.com/` maka akan muncul tampilan sebagai berikut
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

## Soal 11

> Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id. (11) hint: (proxy_pass)

bukan node Eisen kemudian buat location baru yaitu '''~ /its''' yang nantinya akan mengarahkan apabila terdapat link bertulis ...its..., akan secara otomatis mengarah ke its.ac.id

### Script

```sh
mkdir /etc/nginx/rahasisakita
htpasswd -b -c /etc/nginx/rahasisakita/htpasswd netics ajka05
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/konfiq

echo 'upstream worker {
    server 192.171.3.1;
    server 192.171.3.2;
    server 192.171.3.3;
}

server {
    listen 80;
    server_name granz.channel.a05.com www.granz.channel.a05.com;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
	allow 192.171.3.69; #soal 12
        allow 192.171.3.70; #soal12
        allow 192.171.4.167; #soal12
        allow 192.171.4.168; #soal12
        deny all;
        proxy_pass http://worker; #soal8
	auth_basic "Restricted Content"; #soal10
	auth_basic_user_file /etc/nginx/rahasisakita/htpasswd; #soal10
    }

    location ~ /its { #soal11
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}' > /etc/nginx/sites-available/konfiq

ln -s /etc/nginx/sites-available/konfiq /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

### Output

kemudian coba ketik sebagai berikut pada node client

```sh
lynx www.granz.channel.a05.com/its
```

![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

## Soal 12

> Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168.

buka node Eisen kemudian lakukan konfigurasi pada nginx dengan menambahkan
'''sh
allow 192.171.3.69;
allow 192.171.3.70;
allow 192.171.4.167;
allow 192.171.4.168;
deny all;
'''

### Script

```sh
mkdir /etc/nginx/rahasisakita
htpasswd -b -c /etc/nginx/rahasisakita/htpasswd netics ajka05
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/konfiq

echo 'upstream worker {
    server 192.171.3.1;
    server 192.171.3.2;
    server 192.171.3.3;
}

server {
    listen 80;
    server_name granz.channel.a05.com www.granz.channel.a05.com;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
	allow 192.171.3.69; #soal 12
        allow 192.171.3.70; #soal12
        allow 192.171.4.167; #soal12
        allow 192.171.4.168; #soal12
        deny all;
        proxy_pass http://worker; #soal8
	auth_basic "Restricted Content"; #soal10
	auth_basic_user_file /etc/nginx/rahasisakita/htpasswd; #soal10
    }

    location ~ /its { #soal11
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}' > /etc/nginx/sites-available/konfiq

ln -s /etc/nginx/sites-available/konfiq /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

### Output

jika diakses pada node yang tidak disetujui maka anakmuncul sebagai berikut
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)
![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

## Soal 13

> Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern. (13)

mula mula dilakukan setup pada node Denken, kemudian dilakukan konfigurasi pada mysql pada file '''/etc/mysql/my.cnf''' seperti berikut

### Script

```sh
echo '

# The MariaDB configuration file
#
#
[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

[mysqld]
skip-networking=0
skip-bind-address' > /etc/mysql/my.cnf

echo "
#
# These groups are read by MariaDB server.
# Use it for options that only the server (but not clients) should see
#
# See the examples of server my.cnf files in /usr/share/mysql

# this is read by the standalone daemon and embedded servers
[server]

# this is only for the mysqld standalone daemon
[mysqld]

#
# * Basic Settings
#
user                    = mysql
pid-file                = /run/mysqld/mysqld.pid
socket                  = /run/mysqld/mysqld.sock
#port                   = 3306
basedir                 = /usr
datadir                 = /var/lib/mysql
tmpdir                  = /tmp
lc-messages-dir         = /usr/share/mysql
#skip-external-locking

bind-address            = 0.0.0.0
#
character-set-server  = utf8mb4
collation-server      = utf8mb4_general_ci



# this is only for embedded server
[embedded]


# you can put MariaDB-only options here
[mariadb]

[mariadb-10.3]" > /etc/mysql/mariadb.conf.d/50-server.cnf

service mysql restart
service mysql status
```

kemudian lakukan restart mysql nya `service mysql restart`, lalu jalankan perintah dibawah

```sh
mysql -u root -p
Enter password:
CREATE USER 'kelompoka05'@'%' IDENTIFIED BY 'passworda05';
CREATE USER 'kelompoka05'@'localhost' IDENTIFIED BY 'passworda05';
CREATE DATABASE dbkelompoka05;
GRANT ALL PRIVILEGES ON *.* TO 'kelompoka05'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompoka05'@'localhost';
FLUSH PRIVILEGES;
```

### Output

Setelah itu lakukan pengecekan di node salah satu Laravel Worker. dengan mengetik perintah dibawah

```sh
mariadb --host=192.171.2.1 --port=3306 --user=kelompoka05 --password=passworda05 dbkelompoka05 -e "SHOW DATABASES;"
```

![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)

## Soal 14

> Frieren, Flamme, dan Fern memiliki Granz Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer

mula mula lakukan setup yang sesuai dengan soal pada node laravel worker yaitu pada Fern, Flamme, Freiren

### Script

Lakukan install composer

```sh
wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/local/bin/composer
```

Setelah itu install `git` dan lakukan cloning terhadap [resource](https://github.com/martuafernando/laravel-praktikum-jarkom) yang telah diberikan

```sh
apt-get install git -y
cd /var/www && git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd /var/www/laravel-praktikum-jarkom && composer update
```

Setelah melakukan `clone` pada resource tersebut. Sekarang lakukan konfigurasi sebagai berikut pada masing-masing `worker`

```sh
git clone https://github.com/martuafernando/laravel-praktikum-jarkom.git
mv laravel-praktikum-jarkom /var/www/laravel-praktikum-jarkom

cd /var/www/laravel-praktikum-jarkom
composer update
composer install

mv .env.example .env

echo '
APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=192.171.2.1
DB_PORT=3306
DB_DATABASE=dbkelompoka05
DB_USERNAME=kelompoka05
DB_PASSWORD=passworda05

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https
PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"' > /var/www/laravel-praktikum-jarkom/.env

php artisan migrate:fresh
php artisan db:seed --class=AiringsTableSeeder
php artisan key:generate
php artisan jwt:secret
php artisan storage:link

echo '
server {

    listen 8001;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/jarkom_error.log;
    access_log /var/log/nginx/jarkom_access.log;
}' > /etc/nginx/sites-available/laravel-worker

ln -sf /etc/nginx/sites-available/laravel-worker /etc/nginx/sites-enabled/

chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/


service php7.2-fpm start
service nginx restart

lynx localhost:800x
```

800x disesuaikan dengan socket yang di hubungkan, sehingga untuk setiap node dapat dikonfigurasi sebagai berikut

```sh
localhost:8001; # Fern
localhost:8002; # Flamme
localhost:8003; # Frieren
```

kemudian dilakukan konfigurasi pada `nginx` di laravel worker juga

```sh
echo 'server {
    listen 800x;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/implementasi_error.log;
    access_log /var/log/nginx/implementasi_access.log;
}' > /etc/nginx/sites-available/laravel-worker
```

800x, x merupakan port masing-masing Worker.

### Output

ketikkan

```sh
lynx localhost:[PORT]
```

![image](https://github.com/riansyah251641/Jarkom-Modul-3-A05-2023/img/)
