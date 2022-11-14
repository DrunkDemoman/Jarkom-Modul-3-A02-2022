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

