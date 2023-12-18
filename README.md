# K8S Cluster Creation

This repo contains needed scripts to create a K8S cluster with kubeadm & K8S core componentes, our cluster is composed by 1 master node and 2 worker nodes but you can use your own configuration for infrastructure. This repo contains:

`K8S Version: 1.28.0`

- **setupContainerd.sh:** Install & configure `contaiderd` as container runtime to run containers in the cluster.
- **installK8SComponents.sh:** Install `K8S core components` for control plane & data plane, in addition, its configure prevention for K8S components upgrades.
- **clusterInit.sh:** configure the cluster with `Calico` network Add-on and init K8S orchestrator with CIDR 192.168.0.0/16.

## Usage:

### Control & Data Planes:

1. Open a SSH connection with control plane & data plane nodes with the command:

```
ssh -i <YourKey/PemFile> <user>@<public-ip-address>
```

2. In your shell, execute following commands in all control plane & data plane nodes, they are for download script, give execution permissions to it and execute it.

```
wget https://raw.githubusercontent.com/bsantacruz-code/K8S-ClusterCreation/main/setupContainerd.sh

chmod +x setupContainerd.sh

./setupContainerd.sh
```

3. In your shell, execute following commands in all control plane & data plane nodes, they are for download script, give execution permissions to it and execute it.

```
wget https://raw.githubusercontent.com/bsantacruz-code/K8S-ClusterCreation/main/installK8SComponents.sh

chmod +x installK8SComponents.sh

./installK8SComponents.sh
```

Optional: You can execute `https://github.com/bsantacruz-code/K8S-ClusterCreation.git` and then execute files.

### Only in Control Plane:

1. In your control plane node(s) shell, execute following commands to download script, give execution permissions to it and execute it.

```
wget https://raw.githubusercontent.com/bsantacruz-code/K8S-ClusterCreation/main/clusterInit.sh

chmod +x clusterInit.sh

./clusterInit.sh
```

2. Manually, review following command output executed as last step of the script `./clusterInit.sh`:

```
kubeadm token create --print-join-command
```

This command gets the command to join data plane nodes as cluster worker nodes, copy its output for next step. Optionally you can execute it manually.

3. After join nodes to the cluster, validate that `kubectl get nodes` command output returns the public ips from data plane nodes (worker nodes)

### Only in Data Plane nodes:

1. In your data plane node(s) shell, execute the above output command to join the node to the Cluster as Worker Node:

```
sudo kubeadm join ... --token ...
```

This is it. Enjoy K8S!
