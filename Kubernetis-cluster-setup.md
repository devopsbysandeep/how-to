## Master
```bash
yum install -y yum-utils device-mapper-persistent-data lvm2
```
```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
```
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
```
systemctl enable docker --now
```
```
rm /etc/containerd/config.toml
```
```
systemctl restart containerd
```
```
cat >>/etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```
```
sysctl --system
```
+++++ select all command and paste it on master node +++++++
----- <<<-- START select -->> --------
```
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF
```
# Set SELinux in permissive mode (effectively disabling it)
```
sudo setenforce 0
```
```
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```
sudo sed -i '/swap/d' /etc/fstab
```
```
sudo swapoff -a
```
```
sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
```
```
sudo firewall-cmd --permanent --add-port=6443/tcp
```
```
sudo firewall-cmd --permanent --add-port=2379-2380/tcp

```
```
sudo firewall-cmd --permanent --add-port=10250/tcp
```
```
sudo firewall-cmd --permanent --add-port=10251/tcp
```
```
sudo firewall-cmd --permanent --add-port=10252/tcp
```
```
sudo firewall-cmd --permanent --add-port=10255/tcp
```
```
sudo firewall-cmd –reload
```
```
sudo systemctl enable --now kubelet
```
```
systemctl disable firewalld; systemctl stop firewalld
```
----- <<<-- END select -->> --------
yum repolist    ====> you can see Kuberneties repolist
```
kubeadm init
```
OR
```
kubeadm init --apiserver-advertise-address=192.168.1.156 --pod-network-cidr=172.168.0.0/16
```
OUTPUT
```
kubeadm join 192.168.1.156:6443 --token 4hco2r.d75ew........ \
        --discovery-token-ca-cert-hash sha256:25...........
		
```		
-----now you can see kubeadm join command copy it on worker node to join cluster-----
```
mkdir -p $HOME/.kube
```
```
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```
```
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
```
export KUBECONFIG=/etc/kubernetes/admin.conf
```
```
curl https://raw.githubusercontent.com/projectcalico/calico/v3.26.1/manifests/calico.yaml -O
```
```
systemctl restart docker
```
```
kubectl apply -f calico.yaml
```
```
kubeadm token create --print-join-command
```

========== Worker ==================
```
yum install -y yum-utils device-mapper-persistent-data lvm2
```
```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
```
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```
```
systemctl enable docker --now
```
```
rm /etc/containerd/config.toml
```
```
systemctl restart containerd
```
```
cat >>/etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```
```
sysctl --system
```
+++++ select all command and paste it on workers node +++++
----- <<<-- START select -->> --------
```
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF
```
# Set SELinux in permissive mode (effectively disabling it)
```
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
sudo sed -i '/swap/d' /etc/fstab
sudo swapoff -a
```
```
sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
```
```
sudo firewall-cmd --permanent --add-port=10251/tcp
sudo firewall-cmd --permanent --add-port=10255/tcp
firewall-cmd --reload
```
```
systemctl disable firewalld; systemctl stop firewalld
```
```
sudo systemctl enable --now kubelet
```
```
systemctl daemon-reload
```

----- <<<-- END select -->> --------
#yum repolist    ====> you can see Kuberneties repolist

