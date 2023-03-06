# README
This Ansible playbook helps initialize a Kubernetes cluster on a single-host.  Joining more nodes, requires only performing the `sysprep` role and applying the join command.

## Operating system
- Debian 11

## Usage

### Initial Kubernetes node
- Setup the bootstrap node.  This bootstrap node is the first node in the Kubernetes cluster.

```
# Start the playbook
./main.yaml

export KUBECONFIG=/etc/kubernetes/admin.conf

kubectl get nodes
# NAME     STATUS   ROLES    AGE   VERSION
# kube-1   Ready    <none>   33m   v1.25.6

# Print the worker node join command
kubeadm token create --print-join-command
```

### Adding worker nodes
- Run the playbook with only `sysprep`.  Otherwise it will start another bootstrap node.
```
 ./main.yml -t sysprep

# Run the kubeadm join command that the control plane node outputted from `kubeadm token create --print-join-command`
# kubeadm join ...

# Verify on a node that has a KUBECONFIG file for the cluster
kubectl get nodes
# NAME     STATUS   ROLES    AGE    VERSION
# kube-1   Ready    <none>   38m    v1.25.6
# kube-2   Ready    <none>   107s   v1.25.6
```
