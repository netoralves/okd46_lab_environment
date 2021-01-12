# OKD4.6 Cluster Using KVM(PROXMOX)

# Physical Environment:
  Is used a server with 8 physical CPU and 80GB memory and 1TB pool storage (150GB for local-vm and 850GB for vms pool disks), with Proxmox Virtual Environment 6.3-3 installed
  
# Virtual Environment:

|  Name                  |  OS                                  |       Type       |  vCPU  |  RAM  |  Storage  |  IP Address  |
|------------------------|--------------------------------------|------------------|--------|-------|-----------|--------------|
|     okd4-bootstrap     |  Fedora CoreOS 33  			|  Boostrap        |    4   |   16  |    120    |192.168.1.200 |
|  okd4-control-plane-1  |  Fedora CoreOS 33  			|  Master          |    4   |   16  |    120    |192.168.1.201 |
|  okd4-control-plane-2  |  Fedora CoreOS 33  			|  Master          |    4   |   16  |    120    |192.168.1.202 |
|  okd4-control-plane-3  |  Fedora CoreOS 33  			|  Master          |    4   |   16  |    120    |192.168.1.203 |
|     okd4-compute-1     |  Fedora CoreOS 33  			|  Worker          |    4   |   16  |    120    |192.168.1.204 |
|     okd4-compute-2     |  Fedora CoreOS 33      		|  Worker          |    4   |   16  |    120    |192.168.1.205 |
|     okd4-services      |  CentOS 8.3.2011                     |  DNS/LB/Web/NFS  |    4   |    4  |    100    |192.168.1.210 |
|     okd4-pfsence       |  FreeBSD (pfsense 2.4.5-RELEASE-p1)  |  GW/Router/DHCP  |    1   |    1  |    8      |192.168.1.1   |

# Topology
![](images/topology.png?raw=true)

# How-to

## Clone repo on hypervisor
	root@pve-01:~# apt-get install git
	root@pve-01:~# git clone https://github.com/netoralves/okd46_lab_environment.git

## Follow the summary on ansible directory

![01_ansible](infrastructure/01_ansible/?raw=true)

## OKD 4.6

![](images/dashboard.jpg?raw=true)
