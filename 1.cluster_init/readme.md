# k8s-practice

## 安装 minimal centos

- 安装基础工具

```
yum install -y gcc wget net-tools yum-utils
```

- perl(跳过)

```
wget https://www.cpan.org/src/5.0/perl-5.34.1.tar.gz --no-check-certificate
tar -xzf perl-5.34.1.tar.gz
cd perl-5.34.1
./Configure -des -Dprefix=$HOME/localperl
make
make test
make install
```

## 安装docker


```
yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
yum install -y yum-utils
yum-config-manager     --add-repo     https://download.docker.com/linux/centos/docker-ce.repo
yum install -y docker-ce docker-ce-cli containerd.io

systemctl start docker && systemctl enable docker
```

## 修改配置文件


```
# curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io

vi /etc/docker/daemon.json

{
    "registry-mirrors": ["http://f1361db2.m.daocloud.io"],
    "exec-opts": ["native.cgroupdriver=systemd"]
}

systemctl restart docker
```

- 关闭防火墙

```
systemctl stop firewalld && systemctl disable firewalld

```

- 关闭swap

```
swapoff -a

vi /etc/fstab  # 注释掉 swap 行



```

- 关闭 selinux

```
setenforce 0
vi /etc/selinux/config
```
```
SELINUX=disabled

```

```
reboot
sestatus

```

- 允许 iptables 检查桥接流量

```
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system
```

- 安装 kubelet kubectl kubeadm

```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF


yum install -y --nogpgcheck kubelet kubeadm kubectl

sudo systemctl enable --now kubelet

journalctl -xefu kubelet
```

- 初始化master

```
bukeadm -v
kubeadm init --kubernetes-version v1.23.5 --image-repository registry.aliyuncs.com/google_containers --pod-network-cidr=192.168.0.0/16
```


```

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.153.135:6443 --token a00u4l.e0n6kjqv5fko8noq \
        --discovery-token-ca-cert-hash sha256:09f954072f847731d6f347ecbf174efdffbfe209cd95fea2a1168685f0663bbd
```

```
echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> ~/.bash_profile
source ~/.bash_profile
```

- 安装网络插件

```
kubectl apply -f  https://projectcalico.docs.tigera.io/manifests/calico.yaml
```




