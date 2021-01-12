# okd4-computes

okd4-compute directory VM

## Starting the 2 computes nodes:

### Power on the odk4-compute VMs. Press the TAB key to edit the kernel boot options and add the following:

	coreos.inst.install_dev=/dev/sda
	coreos.inst.image_url=http://192.168.1.210:8080/okd4/fcos.raw.xz
	coreos.inst.ignition_url=http://192.168.1.210:8080/okd4/worker.ign

![](../../images/okd4-compute.png?raw=true)

### Waiting conclude download

![](../../images/okd4-compute-download.png?raw=true)

### It is usual for the worker nodes to display the following until the bootstrap process complete:

![](../../images/internal-server-error.png?raw=true)

## bug fix

Make the same process explane ![here](https://github.com/netoralves/okd46_lab_environment/tree/main/infrastructure/04_okd4-bootstrap#bug-fix)
