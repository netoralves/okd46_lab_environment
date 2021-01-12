# End of cluster configuration

## Monitor the bootstrap installation:

### You can monitor the bootstrap process from the okd4-services node:

	openshift-install --dir=install_dir/ wait-for bootstrap-complete --log-level=info

![](../../images/journal_executing.png?raw=true)


### After ending

![](../../images/journal_end.png?raw=true)

![](../../images/logs_end.png?raw=true)

### Once the bootstrap process is complete, which can take upwards of 30 minutes, you can shutdown your bootstrap node. Now is a good time to edit the /etc/haproxy/haproxy.cfg, comment out the bootstrap node, and reload the haproxy service.

	sudo sed '/ okd4-bootstrap /s/^/#/' /etc/haproxy/haproxy.cfg
	sudo systemctl reload haproxy


## Login to the cluster and approve CSRs:

### Now that the masters are online, you should be able to login with the oc client. Use the following commands to log in and check the status of your cluster:

	export KUBECONFIG=~/install_dir/auth/kubeconfig
	oc whoami
	oc get nodes
	oc get csr

![](../../images/get_nodes_master.png?raw=true)

![](../../images/csr.png?raw=true)

### You should only see the master nodes and several CSR’s waiting for approval. Install the jq package to assist with approving multiple CSR’s at once time.

	wget -O jq https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64
	chmod +x jq
	sudo mv jq /usr/local/bin/
	jq --version


### So execute loop to approve certificates:

	oc get csr -ojson | jq -r '.items[] | select(.status == {} ) | .metadata.name' | xargs oc adm certificate approve

![](../../images/csr_approved.png?raw=true)

Obs.: Execute more than one time.

### Now probably worker nodes show in cluster:

![](../../images/worker_nodes.png?raw=true)

### Waiting 10 minutes to load services containers, before the when you list cluster operators you see:

![](../../images/cluster_operators.png?raw=true)

## Access Web Console

### Open your web browser to https://console-openshift-console.apps.lab.okd.local/ (in my case, include this url in my hosts file on workstation connected on "WAN" network) and login as kubeadmin with the password from above:

![](../../images/webconsole-okd4.png?raw=true)

## Copy password on kubeadmin user:

	cat install_dir/auth/kubeadmin-password

![](../../images/webconsole.png?raw=true)

## Persistent Storage

### On okd4-services VM

	sudo dnf install -y nfs-utils
	sudo systemctl enable nfs-server rpcbind
	sudo systemctl start nfs-server rpcbind
	sudo mkdir -p /var/nfsshare/registry
	sudo chmod -R 777 /var/nfsshare
	sudo chown -R nobody:nobody /var/nfsshare

### Create an NFS Export

	echo '/var/nfsshare 192.168.1.0/24(rw,sync,no_root_squash,no_all_squash,no_wdelay)' | sudo tee /etc/exports

### Restart the nfs-server service and add firewall rules:

	sudo setsebool -P nfs_export_all_rw 1
	sudo systemctl restart nfs-server
	sudo firewall-cmd --permanent --zone=public --add-service mountd
	sudo firewall-cmd --permanent --zone=public --add-service rpc-bind
	sudo firewall-cmd --permanent --zone=public --add-service nfs
	sudo firewall-cmd --reload

## Registry configuration:

### Create a persistent volume on the NFS share. Use the registry_py.yaml in okd4_files folder from the git repo:

	oc create -f okd46_lab_environment/infrastructure/07_okd4-conclusion/registry_pv.yaml
	oc get pv

![](../../images/pv.png?raw=true)

### Edit the image-registry operator:

	oc edit configs.imageregistry.operator.openshift.io

### Change the managementState: from Removed to Managed. Under storage: add the pvc: and claim: blank to attach the PV and save your changes automatically:

	managementState: Managed
	storage:
	  pvc:
      	    claim:

![](../../images/edit_registry.png?raw=true)

![](../../images/save_registry.png?raw=true)

### Verify pvs

	[services@okd4-services ~]$ oc get pv
	NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                             STORAGECLASS   REASON   AGE
	registry-pv   100Gi      RWX            Retain           Bound    openshift-image-registry/image-registry-storage                           9m34s
	[services@okd4-services ~]$ du -sh /var/nfsshare/registry
	0	/var/nfsshare/registry

## HTPasswd Setup:

### The easiest way to set up a local user is with htpasswd.

	cd
	cd okd46_lab_environment/infrastructure/07_okd4-conclusion
	htpasswd -c -B -b users.htpasswd admin adminpassword

### Create a secret in the openshift-config project using the users.htpasswd file you generated:

	oc create secret generic htpass-secret --from-file=htpasswd=users.htpasswd -n openshift-config

### Add the identity provider

	oc apply -f htpasswd_provider.yaml

### Follow this commands:

![](../../images/htpasswd.png?raw=true)

### Logout of the OpenShift Console. Then select htpasswd_provider and login with admin and adminpassword credentials:

![](../../images/htpasswd2.png?raw=true)

![](../../images/htpasswd_login.png?raw=true)

![](../../images/dashboard_clear.png?raw=true)

