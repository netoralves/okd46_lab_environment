# OKD4.6 Cluster Using KVM(PROXMOX)

# Physical Environment:
  Is ysed a server with 9 physical CPU and 80GB memory and 1TB disk, installing with Proxmox Virtual Environment 6.3-3
  
# Virtual Environment:

|  Name                  |  Type  |  OS  |  vCPU  |  RAM  |  Storage  |  IP Address  |
|------------------------|--------|------|--------|-------|-----------|--------------|
|     okd4-bootstrap     |        |      |        |       |           |              |
|  okd4-control-plane-1  |        |      |        |       |           |              |
|  okd4-control-plane-2  |        |      |        |       |           |              |
|  okd4-control-plane-3  |        |      |        |       |           |              |
|     okd4-compute-1     |        |      |        |       |           |              |
|     okd4-compute-2     |        |      |        |       |           |              |
|     okd4-services      |        |      |        |       |           |              |
|     okd4-pfsence       |        |      |        |       |           |              |
