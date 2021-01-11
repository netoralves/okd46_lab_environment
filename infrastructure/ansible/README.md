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

[CentOS] (http://mirror.globo.com/centos/8.3.2011/isos/x86_64/CentOS-8.3.2011-x86_64-boot.iso)

[pfSense] (https://nyifiles.pfsense.org/mirror/downloads/pfSense-CE-2.4.5-RELEASE-p1-amd64.iso.gz)

[fedora 33] (https://builds.coreos.fedoraproject.org/prod/streams/stable/builds/33.20201214.3.1/x86_64/fedora-coreos-33.20201214.3.1-live.x86_64.iso)

# Infrastructure

![](../../images/proxmox.png?raw=true)