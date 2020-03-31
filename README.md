# VPN-PiHole-OpenStack-VPS

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
ufw logging low
ufw default allow outgoing
ufw default deny incoming
ufw allow 22

# UFW Firewall aktivieren
ufw --force enable

################
### SECURITY ###
################

# Download fail2ban Jail für ssh_custom_port
apt install fail2ban
wget -O /etc/fail2ban/jail.d/defaults-debian.conf https://pagespeedplus.github.io/ubuntu/etc/fail2ban/jail.d/defaults-debian.conf
fail2ban-client reload && systemctl restart fail2ban.service
