# Jarkom-Modul-2-D01-2021
## Soal 1
EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet.<br><br>
### Jawaban
Untuk Topologi yang sudah kami buat sebagai berikut.
<img src="img/Topologi.png">

Selanjutnya, lakukan konfigurasi setiap node yang akan dijalankan.<br>
* Foosha
<img src="img/ip_foosha.png" width=400 >

* Loguetown
<img src="img/ip_loguetown.png" width=400>

* Alabasta
<img src="img/ip_alabasta.png" width=400>

* Ennieslobby
<img src="img/ip_ennieslobby.png" width=400>

* Water7
<img src="img/ip_water7.png" width=400>

* Skypie
<img src="img/ip_skypie.png" width=400>

Kemudian, semua node diaktifkan dan juga jalankan
```
iptables -t nat -A 
POSTROUTING -o eth0 -j 
MASQUERADE -s 
192.192.0.0/16
```
di Foosa supaya bisa terkoneksi ke internet. Dan jangan lupa jalankan command
```
echo nameserver 
192.168.122.1 > /etc/
resolv.conf
``` 
di semua node.

## Soal 2
Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. 
Kalian diminta Luffy untuk membuat website utama dengan mengakses franky.yyy.com 
dengan alias www.franky.yyy.com pada folder kaizoku.<br><br>
### Jawaban
* Untuk Server EniesLobby<br>
jalankan file script.sh di EniesLobby, dan tunggu sampai download-nya selesai.<br>
Untuk hasil download dari file script.sh sebagai berikut.
```
#/!/bin/bash
    echo nameserver 192.168.122.1 > /etc/resolv.conf
    apt-get update
    apt-get install bind9 -y
```
Lalu jalankan soal 2.sh dengan menggunakan bash di EniesLobby. Dan lakukan konfigurasi pada file `/etc/bind/named.conf.local`
dengan menambahkan.
```
echo 'zone "franky.d01.com" {
        type master;
        file "/etc/bind/kaizoku/franky.d01.com";
};
```
Selanjutnya, menambahkan direktori baru `/etc/bind/kaizoku` dengan konfigurasi `/etc/bind/kaizoku/franky.d01.com`.<br>
```
$TTL    604800
    @       IN      SOA     franky.d01.com. root.franky.d01.com. (
                                2         ; Serial
                            604800         ; Refresh
                            86400         ; Retry
                            2419200         ; Expire
                            604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      franky.d01.com.                                                              24,1    2
    @       IN      A       192.192.2.25%                                                            25,1    5
    www     IN      CNAME   franky.d01.com.0%        
```
Setelah itu lakukan restart bind9 dengan ketik command `service bind9 restart`.

* Untuk Server Loguetown<br> 
Testing 1 : Ping `franky.d01.com`
<img src="img/soal2_testing.png">

Testing 2 : Ping `www.franky.d01.com`
<img src="img/soal2_testing1.png">

## Soal 3
Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com 
yang diatur DNS nya di EniesLobby dan mengarah ke Skypie.<br><br>

### Jawaban
* Untuk Server EniesLobby<br>
Seperti pada nomor 2, jalankan soal3.sh dengan bash di EniesLobby, lalu lakukan restart. Selanjutnya, lakukan edit pada
file `/etc/bind/kaizoku/franky.d01.com` dengan sebagai berikut :
```
$TTL    604800
    @       IN      SOA     franky.d01.com. root.franky.d01.com. (
                                2         ; Serial
                            604800         ; Refresh
                            86400         ; Retry
                            2419200         ; Expire
                            604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      franky.d01.com.
    @       IN      A       192.192.2.2
    www     IN      CNAME   franky.d01.com.
    super   IN      A       192.192.2.4     ; subdomain ke arah IP Skypie
    www.super       IN      CNAME   super.franky.d01.com.
```
Lalu, restart bind9 dengan ketik command `service bind9 restart`.

* Untuk Server Loguetown<br> 
Testing 1 : Ping `franky.d01.com`
<img src="img/soal3_testing.png">

Testing 2 : Ping `www.franky.d01.com`
<img src="img/soal3_testing1.png">

## Soal 4
Buat juga reverse domain untuk domain utama.<br><br>

### Jawaban
* Untuk Server EniesLobby<br>
Jalankan soal4.sh dengan bash di EniesLobby, lalu lakukan restart. Selanjutnya, edit file konfigurasi /etc/bind/named.conf.local
pada file soal4.sh dengan sebagai berikut :
```
zone "franky.d01.com" {
        type master;
        file "/etc/bind/kaizoku/franky.d01.com";
};

zone "2.192.192.in-addr.arpa" {
        type master;
        file "/etc/bind/kaizoku/2.192.192.in-addr.arpa";
};
```
dan juga lakukan konfigurasi pada file `/etc/bind/kaizoku/2.192.192.in-addr.arpa` dengan sebagai berikut :
```
$TTL    604800
    @       IN      SOA     franky.d01.com. root.franky.d01.com. (
                                2         ; Serial
                            604800         ; Refresh
                            86400         ; Retry
                            2419200         ; Expire
                            604800 )       ; Negative Cache TTL                                                              24,1    2
    ;0%                                                            25,1    4
    2.192.192.in-addr.arpa.         IN      NS      franky.d01.com.0%                                                            26,1    6
    2                               IN      PTR     franky.d01.com. ; BYTE niesLobbyE0%
```
Kemudian, jalankan file script.sh di LogueTown, dan tunggu sampai download-nya selesai. Jika sudah,
pindahkankan nameserver ke EniesLobby dengan sebagai berikut:
```
echo 
'nameserver 192.192.2.2'
```
* Untuk Server Loguetown<br> 
Testing : cek dengan `host -t PTR 192.192.2.2`.
<img src="img/soal3_testing.png">

## Soal 5


## Soal 6
Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo

### Jawaban
#### Pada EniesLobby
Pada EnisLobby kami melakukan konfigurasi pada `/etc/bind/kaizoku/franky.d01.com` sebagai berikut

```
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky.d01.com. root.franky.d01.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      franky.d01.com.
@       IN      A       192.192.2.2
www     IN      CNAME   franky.d01.com.
super   IN      A       192.192.2.4     ; subdomain ke arah IP Skypie
www.super IN    CNAME   super.franky.d01.com.
ns1     IN      A       192.192.2.3     ; IP Water7
mecha   IN      NS      ns1             ; delegasi subdomain ke Water7
```
Pada EniesLobby kami hanya menambahkan konfigurasi tersebut, dan tidak merubah konfigurasi pada zone. Selain itu kami juga menambahkan konfigurasi pada `/etc/bind/named.conf.options`

```
options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0s placeholder.

        // forwarders {
        //      0.0.0.0;
        // };

        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        // dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
 ```
 
 #### Pada Water 7
 Pada water 7 pertama sekali menambahkan zone pada file `/etc/bind/named.conf.local` sehingga file tersebut terlihat sebagai berikut
 
 ```
 zone "franky.d01.com" {
        type master;
        notify yes;
        also-notify { 192.192.2.3; }; // IP Water7 DNS Slave
        allow-transfer { 192.192.2.3; }; // IP Water7 DNS Slave dan untuk delegasi subdomain
        file "/etc/bind/kaizoku/franky.d01.com";
};
zone "2.192.192.in-addr.arpa" {
    type master;
    file "/etc/bind/kaizoku/2.192.192.in-addr.arpa";
};

```
Lalu membuat sebuah direktori `mkdir /etc/bind/sunnygo` dan Lakukan konfigurasi pada file `/etc/bind/sunnygo/mecha.franky.d01.com`

```
; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     mecha.franky.d01.com. root.mecha.franky.d01.com. (
                                2         ; Serial
                            604800         ; Refresh
                            86400         ; Retry
                            2419200         ; Expire
                            604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      mecha.franky.d01.com.
    @       IN      A       192.192.2.3     ; IP Water7
    www     IN      CNAME   mecha.franky.d01.com.
```

Setelah itu melakukan restart dengan menggunakan
`service bind9 restart`


#### Testing
ping mecha.franky.d01.com 
![image](https://user-images.githubusercontent.com/73489643/139533618-c85d4dbc-e6d4-49e7-a64a-78c357021a38.png)

ping www.mecha.franky.d01.com 
<img src="img/soal6_testing.png">


## Soal 7
Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie
### Jawaban
#### Pada Water 7

Dalam mengerjakan soal ini, kami hanya menambahkan konfigurasi pada `/etc/bind/sunnygo/mecha.franky.td01.com` menjadi sebagai berikut

```
 ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     mecha.franky.d01.com. root.mecha.franky.d01.com. (
                                2         ; Serial
                            604800         ; Refresh
                            86400         ; Retry
                            2419200         ; Expire
                            604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      mecha.franky.d01.com.
    @       IN      A       192.192.2.3     ; IP Water7
    www     IN      CNAME   mecha.franky.d01.com.
    general IN      A       192.192.2.4     ; subdomain ke IP Skypie
    www.general IN  CNAME   mecha.franky.d01.com.
```
Dapat dilihat bahwa kami membuat sebuah domain general dan alias www untuk general.mecha.franky.d01.com yang mengarah pada Skypie.

Setelah itu lakukan restart dengan menggunakan `service bind9 restart`

#### Testing


## Soal 8
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada `/var/www/franky.yyy.com.`
### Jawaban

## Soal 9
Setelah itu, Luffy juga membutuhkan agar url `www.franky.yyy.com/index.php/home` dapat menjadi menjadi `www.franky.yyy.com/home.`
### Jawaban


## Soal 10
Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada `/var/www/super.franky.yyy.com.`
### Jawaban

### Soal 11
Akan tetapi, pada folder `/public`, Luffy ingin hanya dapat melakukan directory listing saja.
### Jawaban
Pada node `Skypie`, dilakukan konfigurasi pada file `/etc/apache2/sites-available/super.franky.d01.com.conf`sebagai berikut.
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName super.franky.d01.com
    ServerAlias www.super.franky.d01.com
    DocumentRoot /var/www/super.franky.d01.com

    <Directory /var/www/franky.d01.com>
            Options +FollowSymLinks -Multiviews
            AllowOverride All
    </Directory>

    <Directory /var/www/super.franky.d01.com/public/>
            Options +Indexes
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Setelah melakukan konfigurasi, restart apache dengan command
```
service apache2 restart
```

Kemudian, lakukan pengecekan di node `Alabasta` dengan command
```
lynx super.franky.d01.com/public
```

Tampilan `super.franky.d01.com/public` adalah sebagai berikut.
<img src="img/soal11.png">

## Soal 12
Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder `/error` untuk mengganti error kode pada apache
### Jawaban
Pada node `Skypie`, dilakukan konfigurasi pada file `/etc/apache2/sites-available/super.franky.d01.com.conf`sebagai berikut.
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName super.franky.d01.com
    ServerAlias www.super.franky.d01.com
    DocumentRoot /var/www/super.franky.d01.com

    <Directory /var/www/franky.d01.com>
            Options +FollowSymLinks -Multiviews
            AllowOverride All
    </Directory>

    <Directory /var/www/super.franky.d01.com/public/>
            Options +Indexes
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    # Mengarahkan error default apache ke halaman 404.html
    ErrorDocument 404 /error/404.html

    # Mengatur agar halaman error tidak dapat langsung di request oleh client
    <Files "/error/404.html">
        <If "-z %{ENV:REDIRECT_STATUS}">
            RedirectMatch 404 /error/404.html$
        </If>
    </Files>
</VirtualHost>
```

Setelah melakukan konfigurasi, restart apache dengan command
```
service apache2 restart
```

Kemudian, lakukan pengecekan di node `Alabasta` dengan domain yang salah
```
lynx super.franky.d01.com/huehehe
```

Tampilan `super.franky.d01.com/huehehe` adalah sebagai berikut.
<img src="img/soal12.png">

## Soal 13
Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js
### Jawaban
Pada node `Skypie`, dilakukan konfigurasi pada file `/etc/apache2/sites-available/super.franky.d01.com.conf`sebagai berikut.
```
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName super.franky.d01.com
    ServerAlias www.super.franky.d01.com
    DocumentRoot /var/www/super.franky.d01.com

    <Directory /var/www/franky.d01.com>
            Options +FollowSymLinks -Multiviews
            AllowOverride All
    </Directory>

    <Directory /var/www/super.franky.d01.com/public/>
            Options +Indexes
    </Directory>

    # Direktori yang akan dibuat alias
    <Directory /var/www/super.franky.d01.com/public/js>
            Options +Indexes
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    # Mengarahkan error default apache ke halaman 404.html
    ErrorDocument 404 /error/404.html

    # Mengatur agar halaman error tidak dapat langsung di request oleh client
    <Files "/error/404.html">
        <If "-z %{ENV:REDIRECT_STATUS}">
            RedirectMatch 404 /error/404.html$
        </If>
    </Files>

    # membuat alias
    Alias "/js" "/var/www/super.franky.d01.com/public/js"
        
</VirtualHost>
```

Setelah melakukan konfigurasi, restart apache dengan command
```
service apache2 restart
```

Kemudian, lakukan pengecekan di node `Alabasta` dengan domain berikut
```
lynx super.franky.d01.com/js
```

Tampilan `super.franky.d01.com/js` adalah sebagai berikut.
<img src="img/soal13.png">

## Soal 14
Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500
### Jawaban
Pada node `Skypie`, copy file `000-default.conf` ke `general.mecha.franky.d01.conf` sebagai file konfigurasi domain mecha.franky.d01.conf
```
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/general.mecha.franky.d01.conf
```

Pada file `/etc/apache2/sites-available/general.mecha.franky.d01.com.conf` dilakukan konfigurasi sebagai berikut.
```
<VirtualHost *:15000 *:15500>
    ServerAdmin webmaster@localhost
    ServerName general.mecha.franky.d01.com
    ServerAlias www.general.mecha.franky.d01.com
    DocumentRoot /var/www/general.mecha.franky.d01.com

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```

Pada file konfigurasi `/etc/apache2/ports.conf`, tambah port 15000 dan 15500 untuk domain general.mecha.franky.d01.com.conf
```
Listen 80
# Menambahkan port general.mecha.franky.d01.com
Listen 15000
Listen 15500

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>
```

Buat folder DokumenRoot
```
mkdir /var/www/general.mecha.franky.d01.com
```

Copy file yang dibutuhkan ke folder DokumenRoot
```
cp -r /root/Praktikum-Modul-2-Jarkom-main/general.mecha.franky/. /var/www/general.mecha.franky.d01.com
```

Aktifkan konfigurasi yang telah dibuat dengan command
```
a2ensite general.mecha.franky.d01.com
```

Kemudian, lakukan pengecekan di `Alabasta` dengan port 15000
```
lynx general.mecha.franky.d01.com:15000
```
<img src="img/soal14_15000.png">

Pengecekan dengan port 15500
```
lynx general.mecha.franky.d01.com:15500
```
<img src="img/soal14_15500.png">

## Soal 15
Web www.general.mecha.franky.yyy.com dengan autentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy
### Jawaban
Pada node `Skypie` buat konfigurasi username dan password di file `.htpasswd`
```
htpasswd -c /etc/apache2/.htpasswd luffy
```
Kemudian isi password dengan: onepiece
<img src="img/soal15_menambah=pass.png">

Lakukan konfigurasi pada file `/etc/apache2/sites-available/general.mecha.franky.d01.com.conf` agar client tidak dapat mengakses langsung halaman error
```
<VirtualHost *:15000 *:15500>
    ServerAdmin webmaster@localhost
    ServerName general.mecha.franky.d01.com
    ServerAlias www.general.mecha.franky.d01.com
    DocumentRoot /var/www/general.mecha.franky.d01.com

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    # Untuk melakukan setting autentikasi dengan sudah dibuat di file .htpasswd
    <Directory "/var/www/general.mecha.franky.d01.com">
        AuthType Basic
        AuthName "Restricted Content"
        AuthUserFile /etc/apache2/.htpasswd
        Require valid-user
    </Directory>
</VirtualHost>
```

Setelah melakukan konfigurasi, restart apache dengan command
```
service apache2 restart
```

Kemudian, lakukan pengecekan di `Alabasta` dengan port 15000
```
lynx general.mecha.franky.d01.com:15000
```
<img src="img/soal15_15000_username.png">

<img src="img/soal15_15000_password.png">

<img src="img/soal15_15000_hasil.png">

Pengecekan dengan port 15500
```
lynx general.mecha.franky.d01.com:15500
```
<img src="img/soal15_15500_username.png">

<img src="img/soal15_15500_password.png">

<img src="img/soal15_15500_hasil.png">
