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
