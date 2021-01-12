# okd4-services

Config directory to okd4-services.

##Requisites:

	Install CentOS 8.3 with Server with GUI > Guest Agents.

## Installation

![](../../images/boot_centos.png?raw=true)

![](../../images/boot_centos2.png?raw=true)

![](../../images/network_setup.png?raw=true)

![](../../images/enable_ens18.png?raw=true)

![](../../images/enable_ens19.png?raw=true)

![](../../images/mirror_repo.png?raw=true)

![](../../images/Guest_agents.png?raw=true)

![](../../images/centos_storage.png?raw=true)

![](../../images/centos_lvm.png?raw=true)

	services/services

![](../../images/services.png?raw=true)

## Create your root password

![](../../images/root.png?raw=true)

![](../../images/begin_installation.png?raw=true)

![](../../images/installation.png?raw=true)

![](../../images/centos_final.png?raw=true)

![](../../images/centos_final2.png?raw=true)

## I'm sorry, I forgot to change hostname to installation...

![](../../images/hostnamectl.png?raw=true)

## Now, LOGOUT and logins with services user(password is services)

![](../../images/services02.png?raw=true)

## Install git package

![](../../images/install_git.png?raw=true)

## Clone repo
	git clone https://github.com/netoralves/okd46_lab_environment.git

## bind(DNS)
	sudo dnf -y install bind bind-utils

### Copy the named config files and zones:

	sudo cp okd46_lab_environment/infrastructure/03_okd4-services/bind/named.conf /etc/named.conf
	sudo cp okd46_lab_environment/infrastructure/03_okd4-services/bind/named.conf.local /etc/named/
	sudo mkdir /etc/named/zones
	sudo cp okd46_lab_environment/infrastructure/03_okd4-services/bind/db* /etc/named/zones

### Enable and start named:

	sudo systemctl enable named
	sudo systemctl start named
	sudo systemctl status named

### Create firewall rules:

	sudo firewall-cmd --permanent --add-port=53/udp
	sudo firewall-cmd --reload

### Change the DNS on the okd4-service NIC that is attached to the VM Network (not OKD) to 127.0.0.1:

![](../../images/DNS.png?raw=true)

### Restart the network services on the okd4-services VM:

	sudo systemctl restart NetworkManager

### Test DNS on the okd4-services.

	dig okd.local
	dig -x 192.168.1.210

![](../../images/domain.png?raw=true)

![](../../images/dnsresolv.png?raw=true)

## Install HAProxy:

	sudo dnf install haproxy -y


### Copy haproxy config from the git directory:
	sudo cp okd46_lab_environment/infrastructure/03_okd4-services/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg

### Start, enable, and verify HA Proxy service:

	sudo setsebool -P haproxy_connect_any 1
	sudo systemctl enable haproxy
	sudo systemctl start haproxy
	sudo systemctl status haproxy

### Add OKD firewall ports:

	sudo firewall-cmd --permanent --add-port=6443/tcp
	sudo firewall-cmd --permanent --add-port=22623/tcp
	sudo firewall-cmd --permanent --add-service=http
	sudo firewall-cmd --permanent --add-service=https
	sudo firewall-cmd --reload

## Install Apache/HTTPD

	sudo dnf install -y httpd

### Change httpd to listen port to 8080:

	sudo sed -i 's/Listen 80/Listen 8080/' /etc/httpd/conf/httpd.conf

### Enable and Start httpd service/Allow port 8080 on the firewall:

	sudo setsebool -P httpd_read_user_content 1
	sudo systemctl enable httpd
	sudo systemctl start httpd
	sudo firewall-cmd --permanent --add-port=8080/tcp
	sudo firewall-cmd --reload

### Test the webserver:

	curl localhost:8080

## Download the openshift-installer and oc client:

	cd
	wget https://github.com/openshift/okd/releases/download/4.6.0-0.okd-2020-12-12-135354/openshift-install-linux-4.6.0-0.okd-2020-12-12-135354.tar.gz
	wget https://github.com/openshift/okd/releases/download/4.6.0-0.okd-2020-12-12-135354/openshift-client-linux-4.6.0-0.okd-2020-12-12-135354.tar.gz

### Extract the okd version of the oc client and openshift-install:

	tar -zxvf openshift-install-linux-4.6.0-0.okd-2020-12-12-135354.tar.gz
	tar -zxvf openshift-client-linux-4.6.0-0.okd-2020-12-12-135354.tar.gz

### Move the kubectl, oc, and openshift-install to /usr/local/bin and show the version:

	sudo mv kubectl oc openshift-install /usr/local/bin/
	oc version
	openshift-install version

## Setup the openshift-installer:

In the install-config.yaml, you can either use a pull-secret from RedHat or the default of “{“auths”:{“fake”:{“auth”: “bar”}}}” as the pull-secret.

### Generate an SSH key ed25519 if you do not already have one.

	ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519 -C "<your@email.com>"

### Create an install directory and copy the install-config.yaml file:

	cd
	mkdir install_dir
	cp okd46_lab_environment/infrastructure/03_okd4-services/install-config.yaml ./install_dir/

### Edit the install-config.yaml in the install_dir, insert your [pull secret] (https://cloud.redhat.com/) [Clusters > Create > OpenShift Container Platform > Bare Metal > Installer-provisioned infrastructure] and ssh key, and backup the install-config.yaml as it will be deleted in the next step:

![](../../images/rhcloud.png?raw=true)

	vim ./install_dir/install-config.yaml
	cp ./install_dir/install-config.yaml ./install_dir/install-config.yaml.bak

### Generate the Kubernetes manifests for the cluster, ignore the warning:

	openshift-install create manifests --dir=install_dir/

### Modify the cluster-scheduler-02-config.yaml manifest file to prevent Pods from being scheduled on the control plane machines:

	sed -i 's/mastersSchedulable: true/mastersSchedulable: False/' install_dir/manifests/cluster-scheduler-02-config.yml

### Now you can create the ignition-configs:

	openshift-install create ignition-configs --dir=install_dir/


## Host ignition and Fedora CoreOS files on the webserver:

### Create okd4 directory in /var/www/html:

	sudo mkdir /var/www/html/okd4

### Copy the install_dir contents to /var/www/html/okd4 and set permissions:

	sudo cp -R install_dir/* /var/www/html/okd4/
	sudo chown -R apache: /var/www/html/
	sudo chmod -R 755 /var/www/html/

### Test the webserver:

	curl localhost:8080/okd4/metadata.json

### Download the Fedora CoreOS bare-metal bios image and sig files and shorten the file names:
	cd /var/www/html/okd4/
	sudo wget https://builds.coreos.fedoraproject.org/prod/streams/stable/builds/33.20201201.3.0/x86_64/fedora-coreos-33.20201201.3.0-metal.x86_64.raw.xz
	sudo wget https://builds.coreos.fedoraproject.org/prod/streams/stable/builds/33.20201201.3.0/x86_64/fedora-coreos-33.20201201.3.0-metal.x86_64.raw.xz.sig
	sudo mv fedora-coreos-33.20201201.3.0-metal.x86_64.raw.xz fcos.raw.xz
	sudo mv fedora-coreos-33.20201201.3.0-metal.x86_64.raw.xz.sig fcos.raw.xz.sig
	sudo chown -R apache: /var/www/html/
	sudo chmod -R 755 /var/www/html/

