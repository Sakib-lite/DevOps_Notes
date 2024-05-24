###  **Setup Hosts**
###### We can get the ip and vm name by executing ```lxc list``` in the ***host machine***
```
sudo nano /etc/hosts
```
###### Now set host and ip address and save the file
```
k8master x.x.x.x
k8w1 x.x.x.x
```


### **Configure Pre-Requisite Network Plugins**
[Reference](https://v1-29.docs.kubernetes.io/docs/setup/production-environment/container-runtimes/#install-and-configure-prerequisites)
```
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
```

```
sudo modprobe overlay
```

```
sudo modprobe br_netfilter
```

```
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF
```

```
sudo sysctl --system
```

### **Check Installed Network Plugins**
###### If we can see similar output then our pre-requisite configuration is fine
```
lsmod | grep br_netfilter

Output: 
    br_netfilter           20480  0
```
```
lsmod | grep overlay

Output:
    overlay                94208  0
```

```
sysctl net.bridge.bridge-nf-call-iptables net.bridge.bridge-nf-call-ip6tables net.ipv4.ip_forward

Output:
    net.bridge.bridge-nf-call-iptables = 1
    net.bridge.bridge-nf-call-ip6tables = 1
    net.ipv4.ip_forward = 1
```

### **Creating a directory for storing all downloaded files**
```
mkdir ~/my_dir
```

```
cd ~/my_dir
```

### **Install Containerd**
[Reference K8](https://v1-29.docs.kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd)

[Reference GitHub](https://github.com/containerd/containerd/blob/main/docs/getting-started.md)

```
wget https://github.com/containerd/containerd/releases/download/v1.7.16/containerd-1.7.16-linux-amd64.tar.gz
```

```
tar Cxzvf /usr/local containerd-1.7.16-linux-amd64.tar.gz
```

```
sudo mkdir -p /usr/local/lib/systemd/system
```

###### download the containerd.service into this folder
```
wget https://raw.githubusercontent.com/containerd/containerd/main/containerd.service -P /usr/local/lib/systemd/system/
```

```
systemctl daemon-reload
```

```
systemctl enable --now containerd
```

### **Install Runc**
[Reference K8](https://v1-29.docs.kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd)

[Reference GitHub](https://github.com/containerd/containerd/blob/main/docs/getting-started.md)

```
wget https://github.com/opencontainers/runc/releases/download/v1.1.12/runc.amd64
```

```
install -m 755 runc.amd64 /usr/local/sbin/runc
```

### **Install CNI**
[Reference K8](https://v1-29.docs.kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd)

[Reference GitHub](https://github.com/containerd/containerd/blob/main/docs/getting-started.md)

```
wget https://github.com/containernetworking/plugins/releases/download/v1.4.1/cni-plugins-linux-amd64-v1.4.1.tgz
```

```
mkdir -p /opt/cni/bin
```

```
tar Cxzvf /opt/cni/bin cni-plugins-linux-amd64-v1.4.1.tgz
```

```
mkdir /etc/containerd/
```

```
touch /etc/containerd/config.toml
```

```
containerd config default > /etc/containerd/config.toml
```

### **Configuring the systemd cgroup driver**
[Reference](https://v1-29.docs.kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd-systemd)
###### go to /etc/containerd/config.toml and under .runc and runc.options plugin set SystemdCgroup = true

```
sudo nano /etc/containerd/config.toml
```

```
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
 ...
 [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
   SystemdCgroup = true
```

```
sudo systemctl restart containerd
```

### **Overriding the sandbox (pause) image**
###### go to /etc/containerd/config.toml and under io.containerd.grpc.v1.cri plugin set sandbox_image = "registry.k8s.io/pause:3.2"

```
sudo nano /etc/containerd/config.toml
```

```
[plugins."io.containerd.grpc.v1.cri"]
  sandbox_image = "registry.k8s.io/pause:3.2"
```

```
systemctl restart containerd
```

### **installing kubelet, kubeadm, kubectl**
[Reference of permissions](https://v1-29.docs.kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management)

[Reference of packages](https://v1-29.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl)


```
sudo apt-get update
``` 

```
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
```

###### Download key and allow unprivileged APT programs to read this keyring
```
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

```
sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

###### Add k8 repository and helps tools such as command-not-found to work correctly
```
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

```
sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list
```

###### Install kubelet, kubeadm and kubectl

```
sudo apt-get update
```

```
sudo apt-get install -y kubelet kubeadm kubectl
```

###### Stop automatic version updates of targeted packages
```
sudo apt-mark hold kubelet kubeadm kubectl
```

```
sudo systemctl enable --now kubelet
```

# RUN INSIDE MASTER NODE ONLY

### **Network Test**
[Reference](https://v1-29.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#network-setup)
###### Go to this reference link and read this carefully. 

If two or more default gateways are present on the host,
a Kubernetes component will try to use the first one
it encounters that has a suitable global unicast IP address.
While making this choice, the exact ordering of gateways might vary 
between different operating systems and kernel versions


```
ip route show
```

### **initialize cluster**
[Reference](https://v1-29.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#initializing-your-control-plane-node)
 
###### For calico we must need to specify --pod-network-cidr

```
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

[Reference](https://v1-29.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#more-information)

```
mkdir -p $HOME/.kube
```

```
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```

```
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```


### **Pod network configuration**
###### ***We can configure any one of below pod networks***
#### 1. Calico
[Reference](https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart#install-calico)
###### version compatibility with calico
```
calico       |  kubernetes
3.21         |  1.20-1.22
3.22         |  1.21-1.23
3.23         |  1.21-1.23
3.24         |  1.22-1.25
3.25         |  1.23-1.28
3.26         |  1.24-1.28
3.27-latest  |  1.27-1.29
```

```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/tigera-operator.yaml
```

```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/custom-resources.yaml
```

```
watch kubectl get pods -n calico-system
```

```
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```

```
kubectl taint nodes --all node-role.kubernetes.io/master-
```

OR

#### 2. Waveworks

```
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

# RUN INSIDE WORKER NODE ONLY
### **Get token from the master**
###### run below command in the master node

```
kubeadm token create --print-join-command
```

```
Output:
    kubeadm join 10.95.232.84:6443 --token 5mriw2.t6t7jnyc4zla8oti --discovery-token-ca-cert-hash sha256:1517b5f5b6d03653fa32257fc861885a1457883b049d8eb4f6a654413f63330f
```

### **Join with master node**
###### just paste the token that we have found in the previous command
```
kubeadm join 10.95.232.84:6443 --token 5mriw2.t6t7jnyc4zla8oti --discovery-token-ca-cert-hash sha256:1517b5f5b6d03653fa32257fc861885a1457883b049d8eb4f6a654413f63330f
```