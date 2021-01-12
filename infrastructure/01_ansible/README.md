# Ansible
Playbook to provisioned infrastructure on PROXMOX 6.3-3.

# Version
	root@pve-01:~# ansible --version
	ansible 2.10.4
	...

# Download ISOs
	root@pve-01:~# cd /var/lib/vz/template/iso
	root@pve-01:/var/lib/vz/template/iso# ls
	CentOS-8.3.2011-x86_64-boot.iso		       pfSense-CE-2.4.5-RELEASE-p1-amd64.iso
	fedora-coreos-33.20201201.3.0-live.x86_64.iso

![CentOS] (http://mirror.globo.com/centos/8.3.2011/isos/x86_64/CentOS-8.3.2011-x86_64-boot.iso)

![pfSense] (https://nyifiles.pfsense.org/mirror/downloads/pfSense-CE-2.4.5-RELEASE-p1-amd64.iso.gz)

![fedora 33] (https://builds.coreos.fedoraproject.org/prod/streams/stable/builds/33.20201214.3.1/x86_64/fedora-coreos-33.20201214.3.1-live.x86_64.iso)

# Playbook

![](../../images/playbook.png?raw=true)

# Result

## After

![](../../images/proxmox_clean.png?raw=true)

## Before

![](../../images/proxmox_withVMs.png?raw=true)

# Configure pfSense

## Boot okd4-pfsense VM

![02_okd4_pfsense](../02_okd4-pfsense/)

# Configure okd4-services

![03_okd4_services](../03_okd4-services/)

# Configure okd4-bootstrap

![04_okd4_bootstrap](../04_okd4-bootstrap/)

# Configure okd4-controle-plane

![05_okd4_control-plane](../05_okd4-control-plane/)

# Configure okd4-contro

![06_okd4_compute](../06_okd4-compute/)

# Infrastructure

![](../../images/proxmox.png?raw=true)
