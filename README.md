# Jarkom-Modul-2-IT13-2024

## Persiapan

### Script .bashrc Nusantara

```sh
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.70.0.0/16
echo nameserver 192.168.122.1 > /etc/resolv.conf
cat /etc/resolv.conf
```

### Script .bashrc Sriwijaya dan Majapahit

```sh
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
```

### Script .bashrc Clients (HayamWuruk, GajahMada, ThomasAlvaEdison)

```sh
echo nameserver 10.70.1.3 >> /etc/resolv.conf
```

### Buat Topologi

![alt text](image.png)

## Pengerjaan Soal

## NO. 1

### Konfigurasi

### Nusantara (Router)

```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.70.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.70.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.70.3.1
	netmask 255.255.255.0
```

### HayamWuruk (Client)

```
auto eth0
iface eth0 inet static
	address 10.70.1.2
	netmask 255.255.255.0
	gateway 10.70.1.1
```

### Sriwijaya (DNS Master)

```
auto eth0
iface eth0 inet static
	address 10.70.1.3
	netmask 255.255.255.0
	gateway 10.70.1.1
```

### Tanjungkulai (Web Server)

```
auto eth0
iface eth0 inet static
	address 10.70.1.4
	netmask 255.255.255.0
	gateway 10.70.1.1
```

### Bedahulu (Web Server)

```
auto eth0
iface eth0 inet static
	address 10.70.1.5
	netmask 255.255.255.0
	gateway 10.70.1.1
```

### Kotalingga (Web Server)

```
auto eth0
iface eth0 inet static
	address 10.70.1.6
	netmask 255.255.255.0
	gateway 10.70.1.1
```

### Majapahit (DNS Slave)

```
auto eth0
iface eth0 inet static
	address 10.70.2.2
	netmask 255.255.255.0
	gateway 10.70.2.1
```

### Solok (Load Balance)

```
auto eth0
iface eth0 inet static
	address 10.70.2.3
	netmask 255.255.255.0
	gateway 10.70.2.1
```

### GajahMada (Client)

```
auto eth0
iface eth0 inet static
	address 10.70.3.2
	netmask 255.255.255.0
	gateway 10.70.3.1
```

### ThomasAlfaEdison (Client)

```
auto eth0
iface eth0 inet static
	address 10.70.3.3
	netmask 255.255.255.0
	gateway 10.70.3.1
```

## NO. 2

nusantara.sh

```
echo 'zone "sudarsana.it13.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/sudarsana.it13.com";
};' >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it13.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it13.com. root.sudarsana.it13.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it13.com.
@       IN      A       10.70.2.3     ; IP Solok
www     IN      CNAME   sudarsana.it13.com.' > /etc/bind/jarkom/sudarsana.it13.com

service bind9 restart
```

## NO. 3

nusantara.sh

```
echo 'zone "pasopati.it13.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/pasopati.it13.com";
};' >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/pasopati.it13.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it13.com. root.pasopati.it13.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it13.com.
@       IN      A       10.70.1.6     ; IP Kotalingga
www     IN      CNAME   pasopati.it13.com.' > /etc/bind/jarkom/pasopati.it13.com

service bind9 restart
```

## NO. 4

nusantara.sh

```
echo 'zone "rujapala.it13.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/rujapala.it13.com";
};' >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/rujapala.it13.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it13.com. root.rujapala.it13.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it13.com.
@       IN      A       10.70.1.4     ; IP Tanjungkulai
www     IN      CNAME   rujapala.it13.com.' > /etc/bind/jarkom/rujapala.it13.com

service bind9 restart
```

## NO. 5

Script (Clients - HayamWuruk, GajahMada, dan ThomasAlfaEdison)

```
ping sudarsana.it13.com -c 4
ping www.sudarsana.it13.com -c 4

ping pasopati.it13.com -c 4
ping www.pasopati.it13.com -c 4

ping rujapala.it13.com -c 4
ping www.rujapala.it13.com -c 4
```

### Hasil :

### HayamWuruk

![alt text](image-1.png)

![alt text](image-2.png)

### GajahMada

![alt text](image-3.png)

![alt text](image-4.png)

### ThomasAlfaEdison

![alt text](image-5.png)

![alt text](image-6.png)

## NO.6
1. Masuk ke Sriwijaya DNS dengan `nano /etc/bind/named.conf.local`
2. Memasukkan Pointer Record dari Kota Lingga
   ```
   zone "1.70.10.in-addr.arpa" {
    type master;
    file "/etc/bind/it13/1.70.10.in-addr.arpa";
   };
   ```

4. Membuat DNS Record 
```cp /etc/bind/db.local /etc/bind/it13/1.70.10.in-addr.arpa```
5. Melakukan perubahan pada DNS Record
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it13.com. root.pasopati.it13.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
1.70.10.in-addr.arpa.   IN  NS          pasopati.it13.com.
6                       IN  PTR         pasopati.it13.com.
``` 
   5. Lakukan restart pada bind9 `service bind9 restart`
## NO.7 

1. Lakukan perubahan pada Sriwijaya DNS dengan mengedit `/etc/bind/named.conf.local`
2. Lakukan perubahan sebagai berikut
```
zone "sudarsana.it13.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/sudarsana.it13.com";
};

zone "pasopati.it13.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/pasopati.it13.com";
};

zone "rujapala.it13.com" {
    type master;
    notify yes;
    file "/etc/bind/jarkom/rujapala.it13.com";
};

zone "1.70.10.in-addr.arpa" {
    type master;
    file "/etc/bind/it13/1.70.10.in-addr.arpa";
   };
```
3. Restart bind9 `service bind9 restart`
4. Buat DNS Slave pada Majapahit dengan mengedit pada `/etc/bind/named.conf.local`
```
zone "sudarsana.it13.com" {
    type slave;
    masters { 10.70.1.2; };
    file "/var/lib/bind/sudarsana.it13.com";
};

zone "pasopati.it13.com" {
    type slave;
    masters { 10.70.1.2; };
    file "/var/lib/bind/pasopati.it13.com";
};

zone "rujapala.it13.com" {
    type slave;
    masters { 10.70.1.2; };
    file "/var/lib/bind/rujapala.it13.com";
};
```
5. Restart bind9 `service bind9 restart`

## NO. 8 
1. Melakukan perubahan pada DNS Rec dengan menambahkan subdomain cakra pada Bedahulu
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it13.com. root.sudarsana.it13.com. (
                        2023101001      ; Serial
                        604800         ; Refresh
                        86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it13.com.
@       IN      A       10.70.2.3    
www     IN      CNAME   sudarsana.it13.com.
cakra   IN      A       10.70.1.5
```
2. Lakukan Restart bind9 `service bind9 restart`
