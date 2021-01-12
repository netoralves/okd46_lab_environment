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



