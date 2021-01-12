# okd4-control-plane

okd4-control-plane directory VM

## Starting the 3 control-plane nodes:

### Power on the odk4-controle-planes VMs. Press the TAB key to edit the kernel boot options and add the following:

	coreos.inst.install_dev=/dev/sda
	coreos.inst.image_url=http://192.168.1.210:8080/okd4/fcos.raw.xz
	coreos.inst.ignition_url=http://192.168.1.210:8080/okd4/master.ign

![](../../images/okd4-control-plane.png?raw=true)

### Waiting conclude download

![](../../images/okd4-control-plane-download.png?raw=true)

## bug fix

Make the same process explane ![here](https://github.com/netoralves/okd46_lab_environment/tree/main/infrastructure/04_okd4-bootstrap#bug-fix)
