# VPN-PiHole-OpenStack-VPS

- https://github.com/RPiList/specials
- https://docs.pi-hole.net/guides/vpn/installation/

```bash
curl -L https://install.pivpn.io | bash
```

```bash
```

```bash
echo prismbreak > /etc/hostname 
hostname prismbreak && hostnamectl set-hostname prismbreak
```

Dann editiere /etc/hosts.

```bash
nano /etc/hosts
```

Lass es so aussehen:

```bash
127.0.0.1       localhost
80.241.111.111  ngx.page-speed.ninja    ngx

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
80.241.111.111  ngx.page-speed.ninja    ngx
```

Speichere die Datei und prüfe nun Ubuntus Ausgabe.

```bash
hostname 
hostname -f
```

Der erste Befehl gibt den lokalen Hostnamen zurück, während der zweite Befehl den voll qualifizierten Domänennamen (fqdn) anzeigt. Die Ausgabe sollte etwa so aussehen:

```bash
root@server1:# hostname
server1
root@server1:# hostname -f
server1.example.com
```


################
### NETZWERK ###
################

# Standart UFW Firewall Regeln

```bash
ufw logging low
ufw default allow outgoing
ufw default deny incoming
ufw allow 22
```

# UFW Firewall aktivieren

```bash
ufw --force enable
```

################
### SECURITY ###
################

# Download fail2ban Jail für ssh_custom_port

```bash
apt install fail2ban
wget -O /etc/fail2ban/jail.d/defaults-debian.conf https://pagespeedplus.github.io/ubuntu/etc/fail2ban/jail.d/defaults-debian.conf
fail2ban-client reload && systemctl restart fail2ban.service
```

```bash
curl -sSL https://install.pi-hole.net | bash
```

**Anleitung**



### Netzwerk

- Öffentliche IP: 116.203.58.95
- Netzwerk Gateway: 172.31.1.1
- Lokale IP

| Schnittstelle |        IPv4        |             IPv6             |
|:-------------:|:------------------:|:----------------------------:|
|      ens10    |     10.0.0.2/32    |  fe80::8400:ff:fe46:4ca4/64  |
|      eth0     |  116.203.58.95/32	 |    2a01:4f8:c0c:9fd2::1/64   |


ens10

- Zeit seit letzter Aktualisierung: 2s
- Bitrate	Gesamtverbrauch
- Übertragung	NaN undefined/s	8 KB
- Empfang	NaN undefined/s	2 KB

eth0 

- Zeit seit letzter Aktualisierung: 2s
- Bitrate	Gesamtverbrauch
- Übertragung	17 Kb/s	78 MB
- Empfang	13 Kb/s	399 MB

https://docs.pi-hole.net/guides/unbound/

**Update-Datei**

Die Datei `updateroot` öffnen:

```bash
nano /root/updateroot
```

Folgenden Inhalt einfügen:

```bash
#!/bin/bash

if wget -O root.hints https://www.internic.net/domain/named.root ; then
    rm /var/lib/unbound/root.hints
    mv root.hints /var/lib/unbound/
    service unbound restart
fi
```

Die Datei ausführbar machen:

```bash
chmod +x /root/updateroot
```

Cronjobs-Datei öffnen:<br>
```
crontab -e
````

Am Ende der Datei folgende Zeile einfügen:
```
0 0 1 */6 * /root/updateroot &
```

