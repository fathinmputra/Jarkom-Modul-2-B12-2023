# Praktikum Jaringan Komputer
# Modul 2
### Kelompok B12
### Anggota :
|            Nama           |     NRP    |
| ------                    | ------     |
| Darvin Exaudi Simanjuntak | 5025211172 |
| Fathin Muhashibi Putra    | 5025211229 |

## NO. 1
> Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut 
### Penjelasan :

### Screenshot hasil:

## NO. 2

> Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

### Penjelasan :
Melakukan `bash no2.sh` pada DNS Master Yudhistira untuk membuat website utama arjuna.b12.com dan www.arjuna.b12.com

**Yudhistira**
```
echo '
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";
zone "arjuna.b12.com" {
        type master;
        file "/etc/bind/arjuna/arjuna.b12.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/arjuna

cp /etc/bind/db.local /etc/bind/arjuna/arjuna.b12.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     arjuna.b12.com. root.arjuna.b12.com. (
                     2023100901         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      arjuna.b12.com.
@       IN      A       192.184.3.3
www     IN      CNAME   arjuna.b12.com.
@       IN      AAAA    ::1
' > /etc/bind/arjuna/arjuna.b12.com

service bind9 restart
```
- Memasukkan konfigurasi ke `named.conf.local`
- membuat direktori arjuna
- copy `db.local` ke directory arjuna dan ganti namanya menjadi `arjuna.b12.com`
- Terakhir, mengubah data di `arjuna.b12.com` menjadi seperti diatas, kemudian restart bind9.

**Sadewa & Nakula**

Setelah membuat website utama, kita dapat cek website menggunakan ping di `Client Sadewa` atau `Client Nakula` dan cek alias menggunakan host -t CNAME..
```
ping arjuna.b12.com -c 5
host -t CNAME www.arjuna.b12.com
ping www.arjuna.b12.com -c 5
```
### Screenshot hasil:
<img width="600" alt="no2_1" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/3a2d8a20-afaf-4aa1-91e9-f44675dba685">

<img width="600" alt="no2_2" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/a033f0d6-6951-4ee3-8511-58011df6c3f7">

<img width="600" alt="no2_3" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/d100bea0-c504-4a7b-86d0-e9278dcca142">

## NO. 3
> Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

Melakukan `bash no3.sh` pada DNS Master Yudhistira untuk membuat website utama abimanyu.b12.com dan www.abimanyu.b12.com

**Yudhistira**
```
echo '

zone "abimanyu.b12.com" {
        type master;
        file "/etc/bind/abimanyu/abimanyu.b12.com";
};' >> /etc/bind/named.conf.local

mkdir /etc/bind/abimanyu

cp /etc/bind/db.local /etc/bind/abimanyu/abimanyu.b12.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.b12.com. root.abimanyu.b12.com. (
                     2023100901         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.b12.com.
@       IN      A       192.184.3.4
www     IN      CNAME   abimanyu.b12.com.
@       IN      AAAA    ::1
' > /etc/bind/abimanyu/abimanyu.b12.com

service bind9 restart
```
- Memasukkan konfigurasi ke `named.conf.local`
- membuat direktori abimanyu
- copy `db.local` ke directory abimanyu dan ganti namanya menjadi `abimanyu.b12.com`
- Terakhir, mengubah data di `abimanyu.b12.com` menjadi seperti diatas, kemudian restart bind9.


**Sadewa & Nakula**

Setelah membuat website utama, kita dapat cek website menggunakan ping di `Client Sadewa` atau `Client Nakula` dan cek alias menggunakan host -t CNAME.
```
ping abimanyu.b12.com -c 5
host -t CNAME www.abimanyu.b12.com
ping www.abimanyu.b12.com -c 5
```
### Screenshot hasil:
<img width="600" alt="no3_1" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/e0cff728-9b5b-4014-816c-8497046068a7">
<img width="600" alt="no3_2" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/c9444907-ebaf-4b94-a663-3543dfe91a12">
<img width="600" alt="no3_3" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/dc25e667-cdfc-4424-92c3-ff3442dd7fb1">


## NO. 11
> Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

### Penjelasan :

**Abimanyu**

Pada root Abimanyu Webserver dibuat script no11.sh yang antinya perlu di `bash no11.sh`. Berisi :
```
apt-get install apache2 -y
service apache2 start
apt-get install php -y
apt-get install libapache2-mod-php7.0 -y

cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/a$
cp abimanyu.b12.com.conf /etc/apache2/sites-available/abimanyu.b12.com.conf

a2ensite abimanyu.b12.com
service apache2 restart

apt-get install wget
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1aapt-get install unzip
unzip abimanyu.b12.zip

cp -r /root/abimanyu.yyy.com/. /var/www/abimanyu.b12
```

Pada bagian `cp abimanyu.b12.com.conf /etc/apache2/sites-available/abimanyu.b12.com.conf` dari script di atas, isi dari `abimanyu.b12.com.conf` yang disimpan juga pada root sebagai berikut :
```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port t$     # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the requests Host: header to
        # match this virtual host. For the default virtual host (this file) this     # value is not dcisive as it is used as a last resort host regardless.       # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/abimanyu.b12
        ServerName abimanyu.b12.com
        ServerAlias www.abimanyu.b12.com

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn
       ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

**Sadewa & Nakula**

Pada client Sadewa dan Nakula dapat diinputkan command berikut ini untuk menampilkan hasilnya :
```
lynx abimanyu.b12.com
```

### Screenshot hasil:
<img width="407" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/a2a6b8ab-52c0-476b-a75f-afcf22c83d42">

<img width="409" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/f7e3b1fd-eb35-4cda-83af-4207e2395718">


## NO. 12
> Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

### Penjelasan :

### Screenshot hasil:


## NO. 13
> Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

### Penjelasan :

### Screenshot hasil:


## NO. 14
> Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

### Penjelasan :

### Screenshot hasil:


## NO. 15
> Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

### Penjelasan :

### Screenshot hasil:


## NO. 16
> Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 

### Penjelasan :

### Screenshot hasil:


## NO. 17
> Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

### Penjelasan :

### Screenshot hasil:


## NO. 18
> Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

### Penjelasan :

### Screenshot hasil:


## NO. 19
> Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

### Penjelasan :

### Screenshot hasil:


## NO. 20
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

### Penjelasan :

### Screenshot hasil:

