# okd4-bootstrap

okd4-bootstrap directory VM

## Starting the bootstrap node:

### Power on the odk4-bootstrap VM. Press the TAB key to edit the kernel boot options and add the following:

	coreos.inst.install_dev=/dev/sda
	coreos.inst.image_url=http://192.168.1.210:8080/okd4/fcos.raw.xz
	coreos.inst.ignition_url=http://192.168.1.210:8080/okd4/bootstrap.ign

![](../../images/okd4-bootstrap.png?raw=true)

