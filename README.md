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

Topologi yang kami dapat adalah `Topologi no 02`. Berikut Topologi yang sudah dibuat :
![Screenshot 2023-10-15 125120](https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/050a68b4-e101-4d09-89c3-eae8ee2a738d)

### Konfigurasi
**- Pandudewanata (Router)**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.184.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.184.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.184.3.1
	netmask 255.255.255.0
```

**- Sadewa (Client)**
```
auto eth0
iface eth0 inet static
	address 192.184.1.2
	netmask 255.255.255.0
	gateway 192.184.1.1
```

**- Nakula (Client)**
```
auto eth0
iface eth0 inet static
	address 192.184.1.3
	netmask 255.255.255.0
	gateway 192.184.1.1
```

**- Yudhistira (DNS Master)**
```
auto eth0
iface eth0 inet static
	address 192.184.2.2
	netmask 255.255.255.0
	gateway 192.184.2.1
```

**- Werkudara (DNS Slave)**
```
auto eth0
iface eth0 inet static
	address 192.184.3.2
	netmask 255.255.255.0
	gateway 192.184.3.1
```

**- Arjuna (Load Balancer)**
```
auto eth0
iface eth0 inet static
	address 192.184.3.3
	netmask 255.255.255.0
	gateway 192.184.3.1
```

**- Abimanyu (WebServer)**
```
auto eth0
iface eth0 inet static
	address 192.184.3.4
	netmask 255.255.255.0
	gateway 192.184.3.1
```

**- Prabakusuma (WebServer)**
```
auto eth0
iface eth0 inet static
	address 192.184.3.5
	netmask 255.255.255.0
	gateway 192.184.3.1
```

**- Wisanggeni (WebServer)**
```
auto eth0
iface eth0 inet static
	address 192.184.3.6
	netmask 255.255.255.0
	gateway 192.184.3.1
```

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

### Penjelasan :

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

## NO. 4
> Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

### Penjelasan :
Melakukan `bash no4.sh` pada DNS Master Yudhistira untuk membuat subdomain parikesit.abimanyu.b12.com

**Yudhistira**
```
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
@               IN      NS      abimanyu.b12.com.
@               IN      A       192.184.3.4 ; IP Abimanyu
www             IN      CNAME   abimanyu.b12.com.
parikesit       IN      A       192.184.3.4 ; IP Abimanyu
@               IN      AAAA    ::1

' > /etc/bind/abimanyu/abimanyu.b12.com

service bind9 restart
```
- Mengubah konfigurasi `abimanyu.b12.com`
- Menambahkan `A` menuju IP Abimanyu yaitu `192.184.3.4` dengan nama `parikesit`

**Sadewa & Nakula**

Setelah membuat subdomain, kita dapat cek subdomain menggunakan ping di `Client Sadewa` atau `Client Nakula`.
```
ping parikesit.abimanyu.b12.com -c 5
```
### Screenshot hasil:
<img width="600" alt="1" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/272c5dd1-1286-4fdd-b875-906cbcf1030f">
<img width="600" alt="2" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/30a75411-6306-4b14-8e53-d3eecda891e4">

## NO. 5
> Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

### Penjelasan :
Melakukan `bash no5.sh` pada DNS Master Yudhistira untuk reverse domain untuk abimanyu.b12.com.

**Yudhistira**
```
echo '

zone "3.184.192.in-addr.arpa" {
    type master;
    file "/etc/bind/abimanyu/3.184.192.in-addr.arpa";
}; ' >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/abimanyu/3.184.192.in-addr.arpa

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
3.184.192.in-addr.arpa. IN      NS      abimanyu.b12.com.
4                       IN      PTR     abimanyu.b12.com.
'> /etc/bind/abimanyu/3.184.192.in-addr.arpa

service bind9  restart 
```
- Mengubah konfigurasi `named.conf.local` dengan menambahkan zone reverse domainnya.
- Copy db.local menuju abimanyu dengan nama reverse domain
- Kemudian ubah konfigurasi reverse domain 3.184.192.in-addr.arpa seperti di atas.
- Terakhir, restart bind9

**Sadewa & Nakula**

Pertama, install dnsutils pada client Sadewa/Nakula. Dapat dilakukan dengan melakukan `bash no5.sh`
```
echo '
nameserver 192.168.122.1
//nameserver 192.184.2.2
' > /etc/resolv.conf

apt-get update
apt-get install dnsutils

echo '
//nameserver 192.168.122.1
nameserver 192.184.2.2
' > /etc/resolv.conf
```
- Pertama, ubah nameserver ke nameserver awal.
- Kedua, install dnsutils.
- Terakhir, kembalikan nameserver menuju ke IP DNS Master Yudhistira.

Kita dapat cek reverse domain menggunakan host -t PTR di `Client Sadewa` atau `Client Nakula`.
```
host -t PTR 192.184.3.4
```
### Screenshot hasil:
<img width="600" alt="1" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/3549e5b6-8ada-4328-82e8-ae55e652d9a8">
<img width="600" alt="2" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/a717d2d8-8814-46ed-98bb-3d2c059aace3">

## NO. 6
> Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

### Penjelasan :
Melakukan `bash no6.sh` pada DNS Master Yudhistira untuk melakukan konfigurasi di dalam Yudhistira.

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
        notify yes;
        also-notify { 192.184.3.2; };
        allow-transfer { 192.184.3.2; };
        file "/etc/bind/arjuna/arjuna.b12.com";
};


zone "abimanyu.b12.com" {
        type master;
        notify yes;
        also-notify { 192.184.3.2; };
        allow-transfer { 192.184.3.2; };
        file "/etc/bind/abimanyu/abimanyu.b12.com";
};


zone "3.184.192.in-addr.arpa" {
    type master;
    file "/etc/bind/abimanyu/3.184.192.in-addr.arpa";
};
' > /etc/bind/named.conf.local

service bind9 restart
```
- Mengubah konfigurasi `named.conf.local`.
- Menambahkan `notify yes`, `also-notify { IP Werkudara };`, `allow-transfer { Werkudara };` pada zone arjuna dan abimanyu.
- Restart bind9.

**Werkudara**

Melakukan `bash no6.sh` untuk membuat konfigurasi DNS Slave.
```
apt-get update

apt-get install bind9 -y

echo '
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";
zone "arjuna.b12.com" {
    type slave;
    masters { 192.184.2.2; };
    file "/var/lib/bind/arjuna.b12.com";
};

zone "abimanyu.b12.com" {
    type slave;
    masters { 192.184.2.2; };
    file "/var/lib/bind/abimanyu.b12.com";
};
' > /etc/bind/named.conf.local

service bind9 restart
```
- Mengubah konfigurasi `named.conf.local`.
- Menambahkan zone arjuna dan abimanyu
- `type slave` karena Werkudara akan dijadikan DNS Slave dengan master menuju IP Yudhistira.

**Sadewa & Nakula**

Pertama, menambahkan nameserver DNS Slave ke `resolv.conf`. Dapat dilakukan dengan melakukan `bash no6.sh`.
```
echo '
//nameserver 192.168.122.1
nameserver 192.184.2.2
nameserver 192.184.3.2
' > /etc/resolv.conf
```
- Menambahkan nameserver DNS Slave/IP Werkudara.

**Kita akan cek apakah DNS Slave berhasil**

Pertama akan distop bind9 di `Yudhistira`
```
service bind9 stop 
```
Kemudian melakukan ping di `Client Sadewa` atau `Client Nakula`.
```
ping www.abimanyu.b12.com -c 5
```
Jika berhasil, maka DNS Slave sudah benar.

### Screenshot hasil:
<img width="600" alt="1" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/98129bbf-de87-4936-981b-c17fbc770cca">
<img width="600" alt="2" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/c034f7ca-d5bd-471d-ac86-3c7c8f3f72d0">

## NO. 7
> Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

### Penjelasan :

**Yudhistira**
Melakukan `bash no7.sh` untuk memasang konfigurasi di Yudhistira.
```
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
@               IN      NS      abimanyu.b12.com.
@               IN      A       192.184.3.4 ; IP Abimanyu
www             IN      CNAME   abimanyu.b12.com.
parikesit       IN      A       192.184.3.4 ; IP Abimanyu
ns1             IN      A       192.184.3.2 ; IP Werkudara
baratayuda      IN      NS      ns1
@               IN      AAAA    ::1
' > /etc/bind/abimanyu/abimanyu.b12.com

echo '
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

        //=====================================================================$
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //=====================================================================$
        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options

echo '
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";
zone "arjuna.b12.com" {
        type master;
        notify yes;
        also-notify { 192.184.3.2; };
        allow-transfer { 192.184.3.2; };
        file "/etc/bind/arjuna/arjuna.b12.com";
};


zone "abimanyu.b12.com" {
        type master;
        allow-transfer { 192.184.3.2; };
        file "/etc/bind/abimanyu/abimanyu.b12.com";
};


zone "3.184.192.in-addr.arpa" {
    type master;
    file "/etc/bind/abimanyu/3.184.192.in-addr.arpa";
};
' > /etc/bind/named.conf.local

service bind9 restart
```
- Pertama, Mengubah konfigurasi `abimanyu.b12.com` dengan menambahkan ns1 menuju IP Werkudara dan diberi nama baratayuda.
- Kedua, Mengubah konfigurasi `named.conf.options` dengan melakukan comment `dnssec-validation auto;` dan menambahkan `allow-query{any;};`.
- Terakhir, Mengubah konfigurasi `named.conf.local` dengan menghilangkan notify dan also-notify pada zone abimanyu.

**Werkudara**

Melakukan `bash no7.sh` untuk mengatur konfigurasi Werkudara.
```
echo "
options {
        directory \"/var/cache/bind\";

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

        //=====================================================================$
        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
" > /etc/bind/named.conf.options

echo '
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";
zone "arjuna.b12.com" {
    type slave;
    masters { 192.184.2.2; };
    file "/var/lib/bind/arjuna.b12.com";
};

zone "abimanyu.b12.com" {
    type slave;
    masters { 192.184.2.2; };
    file "/var/lib/bind/abimanyu.b12.com";
};
zone "baratayuda.abimanyu.b12.com"{
        type master;
        file "/etc/bind/Baratayuda/baratayuda.abimanyu.b12.com";
};
' > /etc/bind/named.conf.local

mkdir /etc/bind/Baratayuda
cp /etc/bind/db.local /etc/bind/Baratayuda/baratayuda.abimanyu.b12.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.b12.com. root.baratayuda.abimanyu.b$
                     2023100901         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.b12.com.
@       IN      A       192.184.3.4 ; IP Abimanyu
www     IN      CNAME   baratayuda.abimanyu.b12.com.
@       IN      AAAA    ::1
' > /etc/bind/Baratayuda/baratayuda.abimanyu.b12.com

service bind9 restart
```
- Pertama, mengubah konfigurasi `/etc/bind/named.conf.options` persis seperti Yudhistira.
- Kedua, mengubah konfigurasi `named.conf.local` dengan menambahkan zone `baratayuda.abimanyu.b12.com`, kemudian membuat direktori Baratayuda ddan copy db.local ke direktori Baratayuda.
- Terkahir, mengubah konfigurasi file yang dicopy tadi `baratayuda.abimanyu.b12.com`.

**Sadewa & Nakula**

Cek apakah subdomain sudah berjalan atau tidak menggunakan ping di Client Sadewa/Nakula
```
ping baratayuda.abimanyu.b12.com -c 5
ping www.baratayuda.abimanyu.b12.com -c 5
```

### Screenshot hasil:
<img width="600" alt="1" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/6113dc89-bb57-4fda-b063-53097dc990b8">
<img width="600" alt="2" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/4a86f330-7161-411c-ab45-3b9e1043bfdb">

## NO. 8
> Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

### Penjelasan :

**Werkudara**

Melakukan `bash no8.sh` untuk mengatur konfigurasi Werkudara.
```
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.b12.com. root.baratayuda.abimanyu.b$
                     2023100901         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.b12.com.
@       IN      A       192.184.3.4 ; IP Abimanyu
www     IN      CNAME   baratayuda.abimanyu.b12.com.
rjp     IN      A       192.184.3.4 ; IP Abimanyu
www.rjp IN      CNAME   rjp.baratayuda.abimanyu.b12.com.
@       IN      AAAA    ::1
' > /etc/bind/Baratayuda/baratayuda.abimanyu.b12.com

service bind9 restart
```
- Mengubah konfigurasi `baratayuda.abimanyu.b12.com` dengan menambahkan `A` menuju IP abimanyu dan diberi nama `rjp`. Kemudian tambahkan alias `www.rjp` ke `rjp.baratayuda.abimanyu.b12.com`.

**Sadewa & Nakula**

Cek apakah subdomain sudah berjalan atau tidak menggunakan ping di Client Sadewa/Nakula
```
ping rjp.baratayuda.abimanyu.b12.com -c 5
ping www.rjp.baratayuda.abimanyu.b12.com -c 5
```

### Screenshot hasil:
<img width="600" alt="1" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/668e5bc6-b947-4ba6-b9c9-42ffb049346b">
<img width="600" alt="2" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/a6879e4f-a7a9-4869-a6dd-f8b011c312d0">

## NO. 9
> Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

### Penjelasan :

**Prabakusuma & Abimanyu & Wisanggeni**

Melakukan `bash no9.sh` untuk mengatur konfigurasi pada WebServer `Prabakusuma & Abimanyu & Wisanggeni`.
```
mkdir /var/www/arjuna

cp index.php /var/www/arjuna/index.php

echo '
server {

        listen 80;

        root /var/www/arjuna;

        index index.php index.html index.htm;
        server_name www.arjuna.b12.com arjuna.b12.com;

        location / {
                        try_files $uri $uri/ /index.php?$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        }

location ~ /\.ht {
                        deny all;
        }

        error_log /var/log/nginx/arjuna_error.log;
        access_log /var/log/nginx/arjuna_access.log;
}
' > /etc/nginx/sites-available/arjuna

ln -s /etc/nginx/sites-available/arjuna /etc/nginx/sites-enabled

service php7.0-fpm start
rm -rf /etc/nginx/sites-enabled/default
service nginx restart
```
- Pertama, buat direktori arjuna di path /var/www
- Kedua, copy index.php ke direktori arjuna.
- Kemudian, ubah konfigurasi `/etc/nginx/sites-available/arjuna`.
- restart php, remove `/etc/nginx/sites-enabled/default`, dan restart nginx.

Jika sudah, dapat dicek apakah konfigurasi sudah benar apa belum menggunakan `nginx -t`.

Hasil :

<img width="600" alt="3" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/6764e5b8-7a8f-4518-b110-a85a4303fa9f">
<img width="600" alt="4" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/2962626e-f038-4030-af5c-b41ec831665a">
<img width="600" alt="5" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/805c6958-d617-49be-a504-1bbdb9dab9a8">

**Arjuna**

`bash no9.sh` untuk mengubah konfigurasi Arjuna.
```
rm -f /etc/nginx/sites-enabled/lb-arjuna
echo '
# Default menggunakan Round Robin
upstream myweb  {
        server 192.184.3.4 ; #Abimanyu
        server 192.184.3.5 ; #Prabakusuma
        server 192.184.3.6 ; #Wisanggeni
}

server {
        listen 80;
        server_name arjuna.b12.com;

        location / {
        proxy_pass http://myweb;
        }
}
' > /etc/nginx/sites-available/lb-arjuna

ln -s /etc/nginx/sites-available/lb-arjuna /etc/nginx/sites-enabled

service nginx start
service nginx restart
```
- Mengubah konfigurasi `/etc/nginx/sites-available/lb-arjuna`.
- restart nginx.

**Pandudewanata**

`bash no9.sh` untuk mengubah konfigurasi Pandudewanata.
```
echo '
#nameserver 192.168.122.1
' > /etc/resolv.conf
```
- Mengubah konfigurasi `resolv.conf` dengan comment nameserver.

**Sadewa & Nakula**

Melakukan konfigurasi pada Client Sadewa & Nakula dengan eksekusi `bash no9.sh`.
```
echo '
nameserver 192.168.122.1
//nameserver 192.184.2.2
//nameserver 192.184.3.2
' > /etc/resolv.conf

apt-get update
apt-get install lynx

echo '
#nameserver 192.168.122.1
nameserver 192.184.2.2
nanmeserver 192.184.3.2
' > /etc/resolv.conf
```
- Mengubah nameserver menjadi awal aga bisa install.
- Install lynx.
- Ubah lagi nameserver menuju IP DNS Master dan Slave agar bisa membuka domain.

Setelah semua konfigurasi sudah, maka terakhir cek apakah web dapat ditampilkan.
```
lynx http://arjuna.b12.com
```

### Screenshot hasil:
<img width="420" alt="6" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/3623100a-3a12-40fe-9a1a-82c71527fa95">
<img width="420" alt="7" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/d1dfd8fa-5f13-4441-810a-922635b916ff">
<img width="423" alt="8" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/e9b06a91-10ea-4f15-91c9-e3422145ba59">

## NO. 10
> Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

### Penjelasan :

**Arjuna**
`bash no10.sh` untuk konfigurasi Arjuna.

```
echo '
upstream myweb  {
        server 192.184.3.5:8001; #Prabakusuma
        server 192.184.3.4:8002; #Abimanyu
        server 192.184.3.6:8003; #Wisanggeni
}

server {
        listen 80;
        server_name arjuna.b12.com;

        location / {
        proxy_pass http://myweb;
        }
}

server {
        listen 8001;
        server_name arjuna.b12.com:8001;

        location / {
                proxy_pass http://myweb;
        }
}

server {
        listen 8002;
        server_name arjuna.b12.com:8002;

        location / {
                proxy_pass http://myweb;
        }
}

server {
        listen 8003;
        server_name arjuna.b12.com:8003;

        location / {
                proxy_pass http://myweb;
        }
}

' > /etc/nginx/sites-available/lb-arjuna

ln -s /etc/nginx/sites-available/lb-arjuna /etc/nginx/sites-enabled

service nginx restart
```
- Mengubah konfigurasi `/etc/nginx/sites-available/lb-arjuna` dengan menambahkan port 8001-8003 pada masing-masing webserver.
- restart nginx.

**Prabakusuma & Abimanyu & Wisanggeni**

Melakukan `bash no10.sh` untuk mengatur konfigurasi pada WebServer `Prabakusuma & Abimanyu & Wisanggeni`.
```
echo '
server {

        listen 8002;

        root /var/www/arjuna;

        index index.php index.html index.htm;
        server_name www.arjuna.b12.com arjuna.b12.com;

        location / {
                        try_files $uri $uri/ /index.php?$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        }

location ~ /\.ht {
                        deny all;
        }

        error_log /var/log/nginx/arjuna_error.log;
        access_log /var/log/nginx/arjuna_access.log;
}
' > /etc/nginx/sites-available/arjuna

service php7.0-fpm restart
service nginx restart
```
- Mengubah port yang ada di `/etc/nginx/sites-available/arjuna` menjadi 8001-8003 sesuai dengan ketentuan webserver masing masing.
- Contoh di atas merupakan konfigurasi webserver `Abimanyu`, sehingga port diganti dengan `8002`.

**Sadewa & Nakula**

Setelah semua konfigurasi sudah, maka terakhir cek apakah web dapat ditampilkan.
```
lynx http://arjuna.b12.com:8001
lynx http://arjuna.b12.com:8002
lynx http://arjuna.b12.com:8003
```

### Screenshot hasil:
<img width="600" alt="1" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/85867746-f06b-4d3b-b020-511167678dec">
<img width="600" alt="2" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/39a0bddb-5c97-48d0-8358-3adad059ead3">
<img width="600" alt="3" src="https://github.com/fathinmputra/Jarkom-Modul-2-B12-2023/assets/133391111/dfa80a82-56a3-4a3b-8994-bf19bb57c892">

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


