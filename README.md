# Jarkom-Modul-3-A02-2022

## No. 1

### Ostania

#### .bashrc

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
### Ostania
#### .bashrc
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.0.0/16
apt-get update
apt-get install isc-dhcp-relay -y
```

## No. 3-4
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

