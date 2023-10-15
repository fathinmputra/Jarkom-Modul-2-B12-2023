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

Pada root Abimanyu Webserver dibuat script no11.sh yang perlu di `bash no11.sh`. Berisi :
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
**Abimanyu**

Pada root Abimanyu Webserver dibuat script no11.sh yang perlu di `bash no12.sh`. Berisi :
```
echo '
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port t$      # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the requests Host: header to
        # match this virtual host. For the default virtual host (this file) this      # value is not dcisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/abimanyu.b12
        ServerName abimanyu.b12.com
        ServerAlias www.abimanyu.b12.com

        <Directory /var/www/abimanyu.b12/index.php/home>
                Options +Indexes
        </Directory>

        Alias "/home" "/var/www/abimanyu.b12/index.php/home"

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn
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
' > /etc/apache2/sites-available/abimanyu.b12.com.conf

service apache2 restart
```
Pada script tersebut terdapat penambahan bagian :
```
        <Directory /var/www/abimanyu.b12/index.php/home>
                Options +Indexes
        </Directory>
```

dan juga

```
Alias "/home" "/var/www/abimanyu.b12/index.php/home"
```

**Sadewa & Nakula**
Pada client Sadewa dan Nakula dapat diinputkan command berikut ini untuk menampilkan hasilnya :
```
lynx abimanyu.b12.com/home
```

### Screenshot hasil:
<img width="361" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/51ac4944-6e19-4ace-a19b-42cf275d34ef">

<img width="364" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/7652f4b4-c797-45ad-ab31-f0f1f6140b08">


## NO. 13
> Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

### Penjelasan :

**Abimanyu**
Pada root Abimanyu Webserver dibuat script no11.sh yang perlu di `bash no13.sh`. Berisi :
```
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/parikesit.abimanyu.b12.com.conf
cp parikesit.abimanyu.b12.com.conf /etc/apache2/sites-available/parikesit.abimanyu.b12.com.conf

a2ensite parikesit.abimanyu.b12.com

service apache2 restart

apt-get install wget
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1Ldapt-get install unzip
unzip parikesit.abimanyu.b12.zip

cp -r /root/parikesit.abimanyu.yyy.com/. /var/www/parikesit.abimanyu.b12
```

Pada bagian `cp parikesit.abimanyu.b12.com.conf /etc/apache2/sites-available/parikesit.abimanyu.b12.com.conf`, disitu `parikesit.abimanyu.b12.com.conf` disimpan pada root berisi :
```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port t$      # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the requests Host: header to
        # match this virtual host. For the default virtual host (this file) this      # value is not dcisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.b12
        ServerName parikesit.abimanyu.b12.com
        ServerAlias www.parikesit.abimanyu.b12.com

        <Directory /var/www/parikesit.abimanyu.b12/index.php/home>
                Options +Indexes
        </Directory>

        Alias "/home" "/var/www/parikesit.abimanyu.b12/index.php/home"

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn
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
lynx parikesit.abimanyu.b12.com
```

### Screenshot hasil:
<img width="337" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/2f823bab-32de-4ab1-a41c-bcfce9b4c1d5">

<img width="342" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/083e8b06-2ba4-425b-8784-09ed146ab650">


## NO. 14
> Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

### Penjelasan :

**Abimanyu**

Pada root Abimanyu Webserver dibuat script no11.sh yang perlu di `bash no14.sh`. Berisi :
```
mkdir /var/www/parikesit.abimanyu.b12/secret

cp rahasia.html /var/www/parikesit.abimanyu.b12/secret/rahasia.html 

cp parikesit.abimanyu.b12.com.conf /etc/apache2/sites-available/parikesit.abimanyu.b12.com.conf

service apache2 restart
```
Pada bagian `cp rahasia.html /var/www/parikesit.abimanyu.b12/secret/rahasia.html` , isi dari `rahasia.html` yang disimpan di root, yaitu :
```
<!DOCTYPE html>
<html>
<head>
    <title>Cetak Rahasia</title>
</head>
<body>
    <h1>Ini Rahasia!</h1>
</body>
</html>
```

Lalu, pada bagian `cp parikesit.abimanyu.b12.com.conf /etc/apache2/sites-available/parikesit.abimanyu.b12.com.conf` , isi dari `parikesit.abimanyu.b12.com.conf` yang disimpan di root, yaitu :

```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port t$       # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the requests Host: header to
        # match this virtual host. For the default virtual host (this file) this       # value is not dcisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.b12
        ServerName parikesit.abimanyu.b12.com
        ServerAlias www.parikesit.abimanyu.b12.com

        <Directory /var/www/parikesit.abimanyu.b12/public>
                Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.b12/secret>
                Options -Indexes
        </Directory>

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```

Terdapat penambahan di bagian :
```
        <Directory /var/www/parikesit.abimanyu.b12/public>
                Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.b12/secret>
                Options -Indexes
        </Directory>
```

**Sadewa & Nakula**

Pada client Sadewa dan Nakula dapat diinputkan command berikut ini untuk menampilkan hasilnya :
```
lynx parikesit.abimanyu.b12.com/public
```

dan

```
lynx parikesit.abimanyu.b12.com/secret
```

### Screenshot hasil:
```
lynx parikesit.abimanyu.b12.com/public
```

<img width="340" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/a402d0d8-38c2-42ca-80dc-c32c9c902455">


dan

```
lynx parikesit.abimanyu.b12.com/secret
```

<img width="338" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/e3484e04-6d47-4e04-aced-3a5819ac0b53">

<img width="343" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/7c791454-96ac-4ca8-bb9c-73cf315808ec">


## NO. 15
> Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

### Penjelasan :

**Abimanyu**

Pada root Abimanyu Webserver dibuat script no11.sh yang perlu di `bash no15.sh`. Berisi :
```
cp error /etc/apache2/sites-available/parikesit.abimanyu.b12.com.conf

service apache2 restart
```
Pada bagian `cp error /etc/apache2/sites-available/parikesit.abimanyu.b12.com.conf` script di atas, terdapat script bernama `error` yang disimpan pada root untuk di copykan ke `/etc/apache2/sites-available/parikesit.abimanyu.b12.com.conf`. Script `error` tersebut berisi :

```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port t$       # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the requests Host: header to
        # match this virtual host. For the default virtual host (this file) this       # value is not dcisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.b12
        ServerName parikesit.abimanyu.b12.com
        ServerAlias www.parikesit.abimanyu.b12.com

        ErrorDocument 403 /error/403.html
        ErrorDocument 404 /error/404.html

        <Directory /var/www/parikesit.abimanyu.b12/public>
                Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.b12/secret>
                Options -Indexes
        </Directory>

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn
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
lynx parikesit.abimanyu.b12.com/ngaco
```

dan

```
lynx parikesit.abimanyu.b12.com/secret
```

### Screenshot hasil:
```
lynx parikesit.abimanyu.b12.com/ngaco
```
<img width="338" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/91f96190-9f23-41e3-b2fa-050820687565">

<img width="339" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/1010b60c-2984-49ab-96bc-641e999d45e9">

dan

```
lynx parikesit.abimanyu.b12.com/secret
```

<img width="341" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/cee81242-6853-4356-bc1a-6688f62d01c8">

<img width="342" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/ad5f20a6-de47-4843-9f1a-82663fa2a6b5">


## NO. 16
> Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 

### Penjelasan :

**Abimanyu**

Pada root Abimanyu Webserver disimpan script no16.sh yang perlu di `bash no11.sh`. Berisi :

```
cp js /etc/apache2/sites-available/parikesit.abimanyu.b12.com.conf

service apache2 restart
```

Pada bagian `cp js /etc/apache2/sites-available/parikesit.abimanyu.b12.com.conf` , isi dari `js` yang disimpan di root, yaitu :

```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port t$       # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the requests Host: header to
        # match this virtual host. For the default virtual host (this file) this       # value is not dcisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.b12
        ServerName parikesit.abimanyu.b12.com
        ServerAlias www.parikesit.abimanyu.b12.com
        Alias "/js" "/var/www/parikesit.abimanyu.b12/public/js"

        ErrorDocument 403 /error/403.html
        ErrorDocument 404 /error/404.html

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn
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
lynx parikesit.abimanyu.b12.com/js
```

### Screenshot hasil:
<img width="341" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/da2ec454-fdb5-4ccd-9306-091a17bc5b0b">


## NO. 17
> Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

### Penjelasan :

**Abimanyu**

Pada root Abimanyu Webserver disimpan script no17.sh yang perlu di `bash no17.sh`. Berisi :

```
cp rjp /etc/apache2/sites-available/rjp.baratayuda.abimanyu.b12.com.conf

cp ports.conf /etc/apache2/ports.conf

a2ensite rjp.baratayuda.abimanyu.b12.com.conf

service apache2 restart

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1pPSP7yIR05JhSFG67RVzgkb-VcW9vQO6' -O rjp.baratayuda.abimanyu.b12.zip

mkdir /var/www/rjp.baratayuda.abimanyu.b12

cp -r /root/rjp.baratayuda.abimanyu.yyy.com/. /var/www/rjp.baratayuda.abimanyu.b12
```

**Sadewa & Nakula**

Pada client Sadewa dan Nakula dapat diinputkan command berikut ini untuk menampilkan hasilnya :
```
lynx rjp.baratayuda.abimanyu.b12.com:14000
```

dan

```
lynx rjp.baratayuda.abimanyu.b12.com:14400
```

### Screenshot hasil:

```
lynx rjp.baratayuda.abimanyu.b12.com:14000
```
<img width="341" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/c4a1a152-1477-4683-a854-9525bc33d5a3">

dan

```
lynx rjp.baratayuda.abimanyu.b12.com:14400
```
<img width="342" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/bb974ce3-d7a5-4b1c-9792-5b4d0df84319">


## NO. 18
> Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

### Penjelasan :

**Abimanyu**

Pada root Abimanyu Webserver disimpan script no11.sh yang perlu di `bash no18.sh`. Berisi :
```
cp dibatasi /etc/apache2/sites-available/rjp.baratayuda.abimanyu.b12.com.conf 

service apache2 restart

htpasswd -c -b /etc/apache2/.htpasswd Wayang baratayudab12

service apache2 restart
```

Pada bagian `cp dibatasi /etc/apache2/sites-available/rjp.baratayuda.abimanyu.b12.com.conf` , isi dari `dibatasi` yang disimpan di root, yaitu :
```
<VirtualHost *:14000 *:14400>
        # The ServerName directive sets the request scheme, hostname and port t$       # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the requests Host: header to
        # match this virtual host. For the default virtual host (this file) this       # value is not dcisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.b12
        ServerName rjp.baratayuda.abimanyu.b12.com
        ServerAlias www.rjp.baratayuda.abimanyu.b12.com

        <Directory /var/www/rjp.baratayuda.abimanyu.b12>
                AuthType Basic
                AuthName "Dibatasi"
                AuthUserFile /etc/apache2/.htpasswd
                Require valid-user
        </Directory>

        ErrorDocument 404 /error/404.html
        ErrorDocument 403 /error/403.html

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn
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

Terdapat penambahan pada bagian : 
```
        <Directory /var/www/rjp.baratayuda.abimanyu.b12>
                AuthType Basic
                AuthName "Dibatasi"
                AuthUserFile /etc/apache2/.htpasswd
                Require valid-user
        </Directory>
```

**Sadewa & Nakula**

Pada client Sadewa dan Nakula dapat diinputkan command berikut ini untuk menampilkan hasilnya :
```
lynx rjp.baratayuda.abimanyu.b12.com:14000

lynx rjp.baratayuda.abimanyu.b12.com:14400
```

### Screenshot hasil:
<img width="340" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/9ab3fedc-e8cf-4b1b-b70a-2b64c347d259">

<img width="344" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/a1955a06-0abf-4414-be15-033c615e6f7d">

<img width="340" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/4f08cf31-738e-42e0-93f2-05d9665b5e99">

<img width="342" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/342149d3-1975-47d8-9fc9-489c05274b5f">


## NO. 19
> Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

### Penjelasan :
**Abimanyu**

Pada root Abimanyu Webserver disimpan script no19.sh yang perlu di `bash no19.sh`. Berisi :

```
a2enmod rewrite

cp 000-default.conf /etc/apache2/sites-available/000-default.conf 

service apache2 restart
```

Pada bagian `cp 000-default.conf /etc/apache2/sites-available/000-default.conf ` , isi dari `000-default.conf` yang disimpan di root, yaitu :
```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port t$       # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this       # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        RewriteEngine On
        RewriteCond %{HTTP_HOST} !^abimanyu.b12.com$
        RewriteRule /.* http://www.abimanyu.b12.com/ [R]

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

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
lynx 192.184.3.4
```

dan

```
lynx www.abimanyu.b12.com
```

### Screenshot hasil:

```
lynx 192.184.3.4
```
<img width="341" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/3d84f351-0aaa-48c8-8568-97cc866a7fe4">

dan

```
lynx www.abimanyu.b12.com
```

<img width="342" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/9452b1b8-1990-402f-91ac-bf977e665eae">


## NO. 20
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

### Penjelasan :

**Abimanyu**

Pada root Abimanyu Webserver disimpan script no20.sh yang perlu di `bash no20.sh`. Berisi :
```
a2enmod rewrite

cp htaccess /var/www/parikesit.abimanyu.b12/.htaccess

cp png /etc/apache2/sites-available/parikesit.abimanyu.b12.com.conf

service apache2 restart
```

Pada bagian `cp htaccess /var/www/parikesit.abimanyu.b12/.htaccess ` , isi dari `htaccess` yang disimpan di root, yaitu :
```
RewriteEngine On
RewriteCond %{REQUEST_URI} ^/public/images/(.*)abimanyu(.*)
RewriteCond %{REQUEST_URI} !/public/images/abimanyu.png
RewriteRule abimanyu http://parikesit.abimanyu.b12.com/public/images/abimanyu.png$1 [L,R=301] 
```

Lalu, pada bagian `cp png /etc/apache2/sites-available/parikesit.abimanyu.b12.com.conf ` , isi dari `png` yang disimpan di root, yaitu :
```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port $       # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the requests Host: header to
        # match this virtual host. For the default virtual host (this file) ths       # value is not dcisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.b12
        ServerName parikesit.abimanyu.b12.com
        ServerAlias www.parikesit.abimanyu.b12.com
        Alias "/js" "/var/www/parikesit.abimanyu.b12/public/js"

        ErrorDocument 403 /error/403.html
        ErrorDocument 404 /error/404.html

        <Directory /var/www/parikesit.abimanyu.b12/public>
                Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.b12/secret>
                Options -Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.b12>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
        </Directory>

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the # following line enables the CGI configuration for this host only      # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

**Sadewa & Nakula**

Pada client Sadewa dan Nakula dapat diinputkan command berikut ini untuk menampilkan hasilnya :
```
lynx parikesit.abimanyu.b12.com/public/images/ngAsalabimanyuWow
```

### Screenshot hasil:
<img width="341" alt="image" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/103252800/420ee480-7e0a-49f4-8e75-fa12ca40d408">


