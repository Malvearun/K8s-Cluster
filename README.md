# K8s-Cluster


Switch to `Sudo su`
`hostname` check the hostname, if require to change to server and clients   
`vim /etc/hostname`   
`vim /etc/hosts`   
`reboot`   

### On all nodes:

`sudo apt-get update && sudo apt-get install -y apt-transport-https curl`    
`curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -`   


```
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list    
deb https://apt.kubernetes.io/ kubernetes-xenial main    
EOF  
```

`sudo apt-get update`
`sudo apt install docker.io -y`
`sudo apt-get install -y kubelet kubeadm kubectl`
`sudo apt-mark hold kubelet kubeadm kubectl`
`systemctl daemon-reload`
`systemctl restart kubelet`
`apt-get update && apt-get upgrade`


### only on master (if not working change to sudo)
 
`kubeadm init --pod-network-cidr=10.244.0.0/16 -v=9`

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### Worker nodes: change to sudo first
`kubeadm join 172.31.22.206:6443 --token ********* \
    --discovery-token-ca-cert-hash sha256:************`

### Only on master node:
`kubectl get nodes` nodes will not be ready, have to install the cni
`kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml`
`kubectl get nodes` nodes will be ready.
`kubectl run nginx --image=nginx`


### Reference links:

follow the Documentation page for kubernetes installation   

[kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)   
[create a cluster with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)   

