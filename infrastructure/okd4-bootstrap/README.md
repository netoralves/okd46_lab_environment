# okd4-services

Config directory to okd4-services.

Requisites:
	Install CentOS 8.3 with Server with GUI > Guest Agents.

## bind(DNS)
	sudo dnf -y install bind bind-utils

### Copy the named config files and zones:

	sudo cp .bind/named.conf /etc/named.conf
	sudo cp .bind/named.conf.local /etc/named/
	sudo mkdir /etc/named/zones
	sudo cp .bind/db* /etc/named/zones

### Enable and start named:

	sudo systemctl enable named
	sudo systemctl start named
	sudo systemctl status named

### Create firewall rules:

	sudo firewall-cmd --permanent --add-port=53/udp
	sudo firewall-cmd --reload

### Change the DNS on the okd4-service NIC that is attached to the VM Network (not OKD) to 127.0.0.1:

![](../../images/DNS.png?raw=true)
