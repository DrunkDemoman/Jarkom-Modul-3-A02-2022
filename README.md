# Jarkom-Modul-3-A02-2022

+ Ferdinand Putra Gumilang Silalahi - 5025201176
+ Naufal Adli Purnama - 5025201195
+ Bimantara Tito Wahyudi - 5025201227

# Daftar Isi
+ [Nomor 1](#no-1)
+ [Nomor 2](#no-2)
+ [Nomor 3](#no-3)
+ [Nomor 4](#no-4)
+ [Nomor 5](#no-5)
+ [Nomor 6](#no-6)
+ [Nomor 7](#no-7)
+ [Nomor 8](#no-8)
+ [Nomor 9](#no-9)
+ [Nomor 10](#no-10)
+ [Nomor 11](#no-11)
+ [Nomor 12](#no-12)
+ [Nomor 13](#no-13)

## No. 1
Loid bersama Franky berencana membuat peta tersebut dengan kriteria WISE sebagai DNS Server, Westalis sebagai DHCP Server, Berlint sebagai Proxy Server

### Ostania

#### .**bashrc**

```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.0.0/16

```

#### /etc/network/interfaces
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
    address 10.0.1.1
    netmask 255.255.255.0

auto eth2
iface eth2 inet static
    address 10.0.2.1
    netmask 255.255.255.0

auto eth3
iface eth3 inet static
    address 10.0.3.1
    netmask 255.255.255.0

```

### Wise

#### .bashrc
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
```

#### /etc/network/interfaces
```
auto eth0
iface eth0 inet static
    address 10.0.2.2
    netmask 255.255.255.0
    gateway 10.0.2.1

```

### Berlint

#### .bashrc
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install squid -y
```
#### /etc/network/interfaces
```
auto eth0
iface eth0 inet static
    address 10.0.2.3
    netmask 255.255.255.0
    gateway 10.0.2.1
```

### Westalis

#### .bashrc
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y
```

#### /etc/network/interfaces
```
auto eth0
iface eth0 inet static
    address 10.0.2.4
    netmask 255.255.255.0
    gateway 10.0.2.1
```

## No. 2
Ostania sebagai DHCP Relay

### Ostania
#### .bashrc
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.0.0/16
apt-get update
apt-get install isc-dhcp-relay -y
```

## No. 3-4
### Westalis
+ Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155

+ Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85


#### /etc/dhcp/dhcpd.conf
```
ddns-update-style none;
option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;
log-facility local7;

subnet 10.0.1.0 netmask 255.255.255.0 {
    range 10.0.1.50 10.0.1.88;
    range 10.0.1.120 10.0.1.155;
    option routers 10.0.1.1;
    option broadcast-address 10.0.1.255;
}

subnet 10.0.3.0 netmask 255.255.255.0 {
    range 10.0.3.10 10.0.3.30;
    range 10.0.3.60 10.0.3.85;
    option routers 10.0.3.1;
    option broadcast-address 10.0.3.255;
}

subnet 10.0.2.0 netmask 255.255.255.0 {
}

```

#### /etc/default/isc-dhcp-server
```
INTERFACES="eth0"
```

### SSS
#### /etc/network/interfaces
```
auto eth0
iface eth0 inet dhcp
```

### Garden
#### /etc/network/interfaces
```
auto eth0
iface eth0 inet dhcp

```

### Eden
#### /etc/network/interfaces
```
auto eth0
iface eth0 inet dhcp

```
### NewstonCastle
#### /etc/network/interfaces
```
auto eth0
iface eth0 inet dhcp

```

### KemonoPark
#### /etc/network/interfaces
```
auto eth0
iface eth0 inet dhcp

```

## No. 5
Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut.

### WISE
#### /etc/bind/named.conf.options
```
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on { any; };
        listen-on-v6 { any; };
};
```

### Westalis
#### /etc/dhcp/dhcpd.conf
```
ddns-update-style none;
option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;
log-facility local7;

subnet 10.0.1.0 netmask 255.255.255.0 {
    range 10.0.1.50 10.0.1.88;
    range 10.0.1.120 10.0.1.155;
    option routers 10.0.1.1;
    option broadcast-address 10.0.1.255;
    option domain-name-servers 10.0.2.2;
}

subnet 10.0.3.0 netmask 255.255.255.0 {
    range 10.0.3.10 10.0.3.30;
    range 10.0.3.60 10.0.3.85;
    option routers 10.0.3.1;
    option broadcast-address 10.0.3.255;
    option domain-name-servers 10.0.2.2;
}

subnet 10.0.2.0 netmask 255.255.255.0 {
}
```

## No. 6
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit.

### Westalis
#### /etc/dhcp/dhcpd.conf
```
ddns-update-style none;
option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;
log-facility local7;

subnet 10.0.1.0 netmask 255.255.255.0 {
    range 10.0.1.50 10.0.1.88;
    range 10.0.1.120 10.0.1.155;
    option routers 10.0.1.1;
    option broadcast-address 10.0.1.255;
    option domain-name-servers 10.0.2.2;
    default-lease-time 300;
    max-lease-time 6900;
}

subnet 10.0.3.0 netmask 255.255.255.0 {
    range 10.0.3.10 10.0.3.30;
    range 10.0.3.60 10.0.3.85;
    option routers 10.0.3.1;
    option broadcast-address 10.0.3.255;
    option domain-name-servers 10.0.2.2;
    default-lease-time 600;
    max-lease-time 6900;
}

subnet 10.0.2.0 netmask 255.255.255.0 {
}
```

## No. 7
Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13

### Westalis
#### /etc/dhcp/dhcpd.conf
```
ddns-update-style none;
option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;
log-facility local7;

host Eden {
    hardware ethernet 3e:a7:16:d7:67:fe;
    fixed-address 10.0.3.13;
}

subnet 10.0.1.0 netmask 255.255.255.0 {
    range 10.0.1.50 10.0.1.88;
    range 10.0.1.120 10.0.1.155;
    option routers 10.0.1.1;
    option broadcast-address 10.0.1.255;
    option domain-name-servers 10.0.2.2;
    default-lease-time 300;
    max-lease-time 6900;
}

subnet 10.0.3.0 netmask 255.255.255.0 {
    range 10.0.3.10 10.0.3.30;
    range 10.0.3.60 10.0.3.85;
    option routers 10.0.3.1;
    option broadcast-address 10.0.3.255;
    option domain-name-servers 10.0.2.2;
    default-lease-time 600;
    max-lease-time 6900;
}

subnet 10.0.2.0 netmask 255.255.255.0 {
}
```

### Eden
#### /etc/network/interfaces
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 3e:a7:16:d7:67:fe
```

## No. 8
SSS, Garden, dan Eden digunakan sebagai client Proxy agar pertukaran informasi dapat terjamin keamanannya, juga untuk mencegah kebocoran data.

### SSS
#### .bashrc
```
apt-get update
apt-get install lynx -y
export http_proxy="http://10.0.2.3:8080/"
export https_proxy="https://10.0.2.3:443/"
```

### Garden
#### .bashrc
```
apt-get update
apt-get install lynx -y
export http_proxy="http://10.0.2.3:8080/"
export https_proxy="https://10.0.2.3:443/"
```

### Eden
#### .bashrc
```
apt-get update
apt-get install lynx -y
export http_proxy="http://10.0.2.3:8080/"
export https_proxy="https://10.0.2.3:443/"
```

## No. 9
Client hanya dapat mengakses internet diluar (selain) hari & jam kerja (senin-jumat 08.00 - 17.00) dan hari libur (dapat mengakses 24 jam penuh)

### Berlint
#### /etc/squid/acl.conf
```
acl WORKHOURS time MTWHF 08:00-17:00
acl FREEHOURS1 time MTWHF 00:00-07:59
acl FREEHOURS2 time MTWHF 17:01-23:59
acl WEEKENDS time AS 00:00-23:59
```

#### /etc/squid/squid.conf
```
include /etc/squid/acl.conf

sslproxy_flags DONT_VERIFY_PEER
http_port 8080
https_port 443 intercept ssl-bump \
  cert=/etc/squid/certs/squid-ca-cert-key.pem \
  generate-host-certificates=on dynamic_cert_mem_cache_size=16MB
sslcrtd_program /usr/lib64/squid/ssl_crtd -s /var/lib/ssl_db -M 16MB
visible_hostname Berlint

http_access deny WORKHOURS
http_access allow FREEHOURS1
http_access allow FREEHOURS2
http_access allow WEEKENDS
```

## No. 10
Adapun pada hari dan jam kerja sesuai nomor (1), client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP tujuan domain dibebaskan)

### Berlint
#### /etc/squid/acl.conf
```
acl LOID_DOMAIN dstdomain loid-work.com
acl FRANKY_DOMAIN dstdomain franky-work.com
acl WORKHOURS time MTWHF 08:00-17:00
acl FREEHOURS1 time MTWHF 00:00-07:59
acl FREEHOURS2 time MTWHF 17:01-23:59
acl WEEKENDS time AS 00:00-23:59
```

#### /etc/squid/squid.conf
```
include /etc/squid/acl.conf

sslproxy_flags DONT_VERIFY_PEER
http_port 8080
https_port 443 intercept ssl-bump \
  cert=/etc/squid/certs/squid-ca-cert-key.pem \
  generate-host-certificates=on dynamic_cert_mem_cache_size=16MB
sslcrtd_program /usr/lib64/squid/ssl_crtd -s /var/lib/ssl_db -M 16MB
visible_hostname Berlint

http_access allow LOID_DOMAIN WORKHOURS
http_access allow FRANKY_DOMAIN WORKHOURS
http_access deny LOID_DOMAIN FREEHOURS1
http_access deny FRANKY_DOMAIN FREEHOURS2
http_access deny FRANKY_DOMAIN WEEKENDS

http_access allow !LOID_DOMAIN !WORKHOURS
http_access allow !FRANKY_DOMAIN !WORKHOURS
http_access allow !LOID_DOMAIN WEEKENDS
http_access allow !FRANKY_DOMAIN WEEKENDS
```

### WISE
#### /etc/bind/named.conf.local
```
zone "loid-work.com" {
        type master;
        file "/etc/bind/loid/loid-work.com";
};

zone "franky-work.com" {
        type master;
        file "/etc/bind/franky/franky-work.com";
};
```

#### /etc/bind/loid/loid-work.com
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     loid-work.com. root.loid-work.com. (
                     2022110901         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      loid-work.com.
@       IN      A       8.8.8.8
@       IN      AAAA    ::1
```

#### /etc/bind/franky/franky-work.com
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky-work.com. root.franky-work.com. (
                     2022110901         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      franky-work.com.
@       IN      A       8.8.8.8
@       IN      AAAA    ::1
```

## No. 11
Saat akses internet dibuka, client dilarang untuk mengakses web tanpa HTTPS. (Contoh web HTTP: http://example.com)

### Berlint
#### /etc/squid/acl.conf
```
acl HTTPS_PORT port 443
acl LOID_DOMAIN dstdomain loid-work.com
acl FRANKY_DOMAIN dstdomain franky-work.com
acl WORKHOURS time MTWHF 08:00-17:00
acl FREEHOURS1 time MTWHF 00:00-07:59
acl FREEHOURS2 time MTWHF 17:01-23:59
acl WEEKENDS time AS 00:00-23:59
```

#### /etc/squid/squid.conf
```
include /etc/squid/acl.conf

sslproxy_flags DONT_VERIFY_PEER
http_port 8080
https_port 443 intercept ssl-bump \
  cert=/etc/squid/certs/squid-ca-cert-key.pem \
  generate-host-certificates=on dynamic_cert_mem_cache_size=16MB
sslcrtd_program /usr/lib64/squid/ssl_crtd -s /var/lib/ssl_db -M 16MB
visible_hostname Berlint

http_access deny !HTTPS_PORT

http_access allow LOID_DOMAIN WORKHOURS
http_access allow FRANKY_DOMAIN WORKHOURS
http_access deny LOID_DOMAIN FREEHOURS1
http_access deny LOID_DOMAIN FREEHOURS2
http_access deny LOID_DOMAIN WEEKENDS
http_access deny FRANKY_DOMAIN FREEHOURS1
http_access deny FRANKY_DOMAIN FREEHOURS2
http_access deny FRANKY_DOMAIN WEEKENDS

http_access allow !LOID_DOMAIN !WORKHOURS
http_access allow !FRANKY_DOMAIN !WORKHOURS
http_access allow !LOID_DOMAIN WEEKENDS
http_access allow !FRANKY_DOMAIN WEEKENDS
```

## No. 12
Agar menghemat penggunaan, akses internet dibatasi dengan kecepatan maksimum 128 Kbps pada setiap host (Kbps = kilobit per second; lakukan pengecekan pada tiap host, ketika 2 host akses internet pada saat bersamaan, keduanya mendapatkan speed maksimal yaitu 128 Kbps)

### Berlint
#### /etc/squid/squid.conf
```
include /etc/squid/acl.conf
include /etc/squid/acl-bandwidth.conf

sslproxy_flags DONT_VERIFY_PEER
http_port 8080
https_port 443 intercept ssl-bump \
  cert=/etc/squid/certs/squid-ca-cert-key.pem \
  generate-host-certificates=on dynamic_cert_mem_cache_size=16MB
sslcrtd_program /usr/lib64/squid/ssl_crtd -s /var/lib/ssl_db -M 16MB
visible_hostname Berlint

http_access deny !HTTPS_PORT

http_access allow LOID_DOMAIN WORKHOURS
http_access allow FRANKY_DOMAIN WORKHOURS
http_access deny LOID_DOMAIN FREEHOURS1
http_access deny LOID_DOMAIN FREEHOURS2
http_access deny LOID_DOMAIN WEEKENDS
http_access deny FRANKY_DOMAIN FREEHOURS1
http_access deny FRANKY_DOMAIN FREEHOURS2
http_access deny FRANKY_DOMAIN WEEKENDS

http_access allow !LOID_DOMAIN !WORKHOURS
http_access allow !FRANKY_DOMAIN !WORKHOURS
http_access allow !LOID_DOMAIN WEEKENDS
http_access allow !FRANKY_DOMAIN WEEKENDS
```

#### /etc/squid/acl-bandwidth.conf
```
delay_pools 1
delay_class 1 1
delay_access 1 allow all
delay_parameters 1 16000/64000
```

## No. 13
Setelah diterapkan, ternyata peraturan nomor (4) mengganggu produktivitas saat hari kerja, dengan demikian pembatasan kecepatan hanya diberlakukan untuk pengaksesan internet pada hari libur

### Berlint
#### /etc/squid/acl.conf
```
acl HTTPS_PORT port 443
acl LOID_DOMAIN dstdomain loid-work.com
acl FRANKY_DOMAIN dstdomain franky-work.com
acl WORKHOURS time MTWHF 08:00-17:00
acl FREEHOURS1 time MTWHF 00:00-07:59
acl FREEHOURS2 time MTWHF 17:01-23:59
acl WEEKENDS time AS 00:00-23:59
```

#### /etc/squid/acl-bandwidth.conf
```
include /etc/squid/acl.conf
delay_pools 1
delay_class 1 1
delay_access 1 allow WEEKENDS
delay_parameters 1 16000/64000
```

#### /etc/squid/squid.conf
```
include /etc/squid/acl.conf
include /etc/squid/acl-bandwidth.conf

sslproxy_flags DONT_VERIFY_PEER
http_port 8080
https_port 443 intercept ssl-bump \
  cert=/etc/squid/certs/squid-ca-cert-key.pem \
  generate-host-certificates=on dynamic_cert_mem_cache_size=16MB
sslcrtd_program /usr/lib64/squid/ssl_crtd -s /var/lib/ssl_db -M 16MB
visible_hostname Berlint

http_access deny !HTTPS_PORT

http_access allow LOID_DOMAIN WORKHOURS
http_access allow FRANKY_DOMAIN WORKHOURS
http_access deny LOID_DOMAIN FREEHOURS1
http_access deny LOID_DOMAIN FREEHOURS2
http_access deny LOID_DOMAIN WEEKENDS
http_access deny FRANKY_DOMAIN FREEHOURS1
http_access deny FRANKY_DOMAIN FREEHOURS2
http_access deny FRANKY_DOMAIN WEEKENDS

http_access allow !LOID_DOMAIN !WORKHOURS
http_access allow !FRANKY_DOMAIN !WORKHOURS
http_access allow !LOID_DOMAIN WEEKENDS
http_access allow !FRANKY_DOMAIN WEEKENDS
```

