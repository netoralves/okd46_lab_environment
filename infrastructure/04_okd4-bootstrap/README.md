# okd4-bootstrap

okd4-bootstrap directory VM

## Starting the bootstrap node:

### Power on the odk4-bootstrap VM. Press the TAB key to edit the kernel boot options and add the following:

	coreos.inst.install_dev=/dev/sda
	coreos.inst.image_url=http://192.168.1.210:8080/okd4/fcos.raw.xz
	coreos.inst.ignition_url=http://192.168.1.210:8080/okd4/bootstrap.ign

![](../../images/okd4-bootstrap.png?raw=true)

### Waiting conclude download

![](../../images/okd4-bootstrap-download.png?raw=true)

### Before boot, follow logs on bootstrap VM

	ssh core@192.168.1.200
	journalctl -b -f -u release-image.service -u bootkube.service

## bug fix

### After reboot, verify symlink on resolv.conf

![](../../images/symlink.png?raw=true)

### If this happen is because some permission on SELINUX is don't work correctly (If someone know how fix it keeping systemd-resolved execution, please let me know).
![](../../images/systemd-resolved.png?raw=true)

### For a while disable systemd-resolved

	sudo systemctl stop systemd-resolved
	sudo systemctl disable systemd-resolved

### Remove remove symlink and restart NetworkManager

	sudo rm -f /etc/resolv.conf
	sudo systemctl restart NetworkManager

![](../../images/resolvconf.png?raw=true)
