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
