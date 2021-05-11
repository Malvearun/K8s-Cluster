# K8s-Cluster

hostname
vim /etc/hostname
vim /etc/hosts
reboot

******On all nodes:

sudo apt-get update && sudo apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -


cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update
sudo apt install docker.io -y
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
systemctl daemon-reload
systemctl restart kubelet
apt-get update && apt-get upgrade


******only on master
 
kubeadm init --pod-network-cidr=10.244.0.0/16 -v=9

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

******Worker nodes: change to sudo first
kubeadm join 172.31.22.206:6443 --token cumxsg.yafe8ds6x9qs6e4c \
    --discovery-token-ca-cert-hash sha256:5c97619c811c67f183d720033ce4eb1206a6ebc14c3412b71696007ae7a079b1

******Only on master node:
kubectl get nodes
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
kubectl get nodes
kubectl run nginx --image=nginx
