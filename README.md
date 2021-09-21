# K8S-Multinode-Cluster using Kubeadm for Ubuntu
# All Nodes
```
apt-get -y update
apt-get install -y docker.io
apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm kubectl
kubelet --version
kubeadm version
kubectl version
apt-mark hold kubelet kubeadm kubectl
swapoff -a
```
# Master/Leader Node
```
ifconfing/ip -a/ip address
export MASTER_IP=<IP-of-Node>
kubeadm init --apiserver-advertise-address=${MASTER_IP} --pod-network-cidr=10.244.0.0/16
```
# Join worker nodes to the Master/Leader node
```
kubeadm join 206.189.134.39:6443 --token dxxfoj.a2zzwbfrjejzir4h \
    --discovery-token-ca-cert-hash sha256:110e853989c2401b1e54aef6e8ff0393e05f18d531a75ed107cf6c05ca4170eb
```
# Install CNI plugin
The below command can be run on the leader node to install the CNI plugin
```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
# Setting up Kubeconfig file
To start using your cluster, you need to run the following as a regular user:
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

```
# Show Pods
```
kubectl get pods --all-namespaces
watch kubectl get pods --all-namespaces
```
# Troubleshooting Guide
Create this file "daemon.json" in the directory "/etc/docker" and add the following
```
cd /etc/docker/daemon.json
{
"exec-opts": ["native.cgroupdriver=systemd"]
}
systemctl restart docker
```
# Additional Info
```
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
netstat -ltnp | grep -w ":10250"
netstat -lnp | grep 10250
kill 9823
```
