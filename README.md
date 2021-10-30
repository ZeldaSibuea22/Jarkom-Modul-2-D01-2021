# Jarkom-Modul-2-D01-2021

## Soal 1
EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet.<br><br>
### Jawaban
Untuk Topologi yang sudah kami buat sebagai berikut.
<img src="img/Topologi.png">


## Soal 2


## Soal 3


## Soal 4


## Soal 5


## Soal 6
Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo

### Jawaban

## Soal 7
Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie
### Jawaban

## Soal 8
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada `/var/www/franky.yyy.com.`
### Jawaban

## Soal 9
Setelah itu, Luffy juga membutuhkan agar url `www.franky.yyy.com/index.php/home` dapat menjadi menjadi `www.franky.yyy.com/home.`
### Jawaban


## Soal 10
Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada `/var/www/super.franky.yyy.com.`
### Jawaban

## Soal 11
Akan tetapi, pada folder `/public`, Luffy ingin hanya dapat melakukan directory listing saja.
### Jawaban


## Soal 12
Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder `/error` untuk mengganti error kode pada apache
### Jawaban


## Soal 13
Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js
### Jawaban

