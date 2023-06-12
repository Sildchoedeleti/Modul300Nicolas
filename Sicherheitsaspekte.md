In meinem Vagrant File habe ich verschiedene Sicherheitsaspekte angewendet:



### Reverse Proxy
```
# Reverse Proxy-Konfiguration
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo sh -c 'echo "ProxyPass \"/app1/\" \"http://backend1:8080/\"" >> /etc/apache2/sites-available/000-default.conf'
sudo sh -c 'echo "ProxyPassReverse \"/app1/\" \"http://backend1:8080/\"" >> /etc/apache2/sites-available/000-default.conf'
sudo sh -c 'echo "ProxyPass \"/app2/\" \"http://backend2:8080/\"" >> /etc/apache2/sites-available/000-default.conf'
sudo sh -c 'echo "ProxyPassReverse \"/app2/\" \"http://backend2:8080/\"" >> /etc/apache2/sites-available/000-default.conf'
```

Diese Abschnitte konfigurieren den Apache-Reverse-Proxy, der Anfragen für `/app1/` an `http://backend1:8080/` und Anfragen für `/app2/` an 
`http://backend2:8080/` weiterleitet.

### Firewall
```
# Firewall-Regeln erstellen
sudo iptables -F
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
# Weitere Firewall-Regeln hinzufügen...
sudo iptables-save | sudo tee /etc/iptables/rules.v4
```

Diese Abschnitte erstellen und konfigurieren die Firewall-Regeln. Hier werden alle eingehenden Verbindungen standardmäßig blockiert, 
außer für den SSH-Zugriff und bereits etablierte/verwandte Verbindungen.

### User und Gruppen mit Berechtigungen
```
# Benutzergruppen und Benutzer erstellen
sudo groupadd group1
sudo groupadd group2
sudo groupadd group3
sudo groupadd group4

sudo useradd -m -s /bin/bash -G group1 user1
sudo useradd -m -s /bin/bash -G group2 user2
sudo useradd -m -s /bin/bash -G group3 user3
sudo useradd -m -s /bin/bash -G group4 user4

# Beispielhafte Benutzerrechte
sudo chown -R user1:group1 /var/www/app1
sudo chmod -R 750 /var/www/app1

sudo chown -R user2:group2 /var/www/app2
sudo chmod -R 750 /var/www/app2
```

Diese Abschnitte erstellen Benutzergruppen und Benutzer mit den angegebenen Namen. Es werden vier Gruppen (group1, group2, group3, group4)
und vier Benutzer (user1, user2, user3, user4) erstellt. Anschließend werden Beispielrechte für die Verzeichnisse `/var/www/app1` und `/var/www/app2`
zugewiesen.

### SSH Zugriff
```
# Portweiterleitung für SSH
config.vm.network "forwarded_port", guest: 22, host: 22, host_ip: "127.0.0.1", auto_correct: true


```

Diese Zeile konfiguriert die Portweiterleitung, um den SSH-Zugriff auf den Container zu ermöglichen. Der SSH-Port (Port 22) wird vom Host auf den Gast
(Container) weitergeleitet.

