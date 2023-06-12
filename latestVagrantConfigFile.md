Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "ContainerModul300"
    vb.memory = "4096"
    vb.cpus = 2
  end

  # Portweiterleitung für SSH
  config.vm.network "forwarded_port", guest: 22, host: 22, host_ip: "127.0.0.1", auto_correct: true

  # Alle nicht-öffentlichen Ports blockieren
  config.vm.network :forwarded_port, guest: 1, host: 1, protocol: "tcp", auto_correct: true
  config.vm.network :forwarded_port, guest: 1, host: 1, protocol: "udp", auto_correct: true

  config.vm.provision "shell", inline: <<-SHELL
    # Firewall-Regeln erstellen
    sudo iptables -F
    sudo iptables -P INPUT DROP
    sudo iptables -P FORWARD DROP
    sudo iptables -P OUTPUT ACCEPT
    sudo iptables -A INPUT -i lo -j ACCEPT
    sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
    sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
    # Weitere Firewall-Regeln hinzufügen...

    # Reverse Proxy-Konfiguration
    sudo a2enmod proxy
    sudo a2enmod proxy_http
    sudo sh -c 'echo "ProxyPass \"/app1/\" \"http://backend1:8080/\"" >> /etc/apache2/sites-available/000-default.conf'
    sudo sh -c 'echo "ProxyPassReverse \"/app1/\" \"http://backend1:8080/\"" >> /etc/apache2/sites-available/000-default.conf'
    sudo sh -c 'echo "ProxyPass \"/app2/\" \"http://backend2:8080/\"" >> /etc/apache2/sites-available/000-default.conf'
    sudo sh -c 'echo "ProxyPassReverse \"/app2/\" \"http://backend2:8080/\"" >> /etc/apache2/sites-available/000-default.conf'

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

    # Firewall-Regeln speichern
    sudo iptables-save | sudo tee /etc/iptables/rules.v4

    sudo apt-get update
    sudo apt-get -y install apache2
    sudo systemctl restart apache2
  SHELL

  config.vm.network "forwarded_port", guest: 80, host: 8080
end
