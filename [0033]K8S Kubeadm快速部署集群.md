## K8S Kuberadm 快速部署集群

[TOC]

### kubeadm 搭建简介

kubeadm 是官方社区推出的一个用于快速部署 kubernetes 集群的工具

kubeadm 通过两条指令完成 kubernetes 集群部署

```
# 创建一个 Master 节点
kubeadmin init

# 将一个 Node 节点加入到当前的集群中
kubeadm join <Master 节点的 IP 和端口>
```

#### 安装要求

满足以下几个条件：

- 一台或多台机器，操作系统 CentOS
- 硬件配置：2G+2cpu+30GB disk
- 打通外网，可拉去镜像
- 禁止 swap 分区

#### 准备环境

![image-20230123154335728](https://raw.githubusercontent.com/MarcLan/pic/main/image-20230123154335728.png)

```
# 关闭防火墙
systemctl stop firewalld
systemctl disable firewalld

# 关闭 selinux
sed -i 's/enforcing/disabled/' /etc/selinux/config #永久
setenforce 0 # 临时

# 关闭 swap
swapoff -a # 临时
sed -ri 's/."swap."/#&/' /etc/fstab # 永久

# 根据规划设置主机名
hostnamectl set-hostname <hostname>

# 在 master 添加 hosts
cat >> /etc/hosts << EOF
IP master
IP node1
IP node2
EOF

# 将桥接的 IPv4 流量传递到 IPtables 链
cat > /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system # 生效

# 时间同步
yum install ntpdate -y
ntpdate time.windows.com
```

#### 所有节点安装 Docker/Kubeadm/kubelet

kubernetes 默认安装 CRI（容器运行时）为 Docker，引起先安装 Docker--[0034]

#### 安装 Docker

```
wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo -O /etc/yum.repos.d/docker-ce.repo

yum -y install docker-ce-18.06.1.ce-3.el7
systemctl enable docker && systemctl start docker
docker --version

cat > /etc/docker/daemon.json << EOF
{
	"registry-mirrors":["https://b9pmyelo.mirror.aliyuncs.com"]
}
EOF
```

#### 添加阿里云 YUM 软件源--[0035]

```
cat > /etc/yum.repos.d/kubernetes.repo << EOF
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```

#### 安装kubeadm，kubelet，kubectl

```
yum install -y kubelet kubeadm kubectl
systemctl enable kubelet
```

#### 部署Kubernetes Master

```
kubeadm init \
--apiserver-advertise-address=192.168.0.241 \
--image-repository registry.aliyumcs.com/google_containers \
--service-cidr=10.96.0.0/12 \
--pod-network-cidr=10.244.0.0/16

--kubernetes-version v.1.26.1 \
```

#### 部署 CNI 网络插件

```
wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

若默认镜像地址无法访问，sed命令修改为 docker hub 镜像仓库

```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

kubectl get pods -n kube-system
```

#### 测试 kubernetes 集群

创建一个 pod，验证是否正常运行

```
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=NodePort

kubectl get pod -o wide # 查看nginx运行在哪个node上
curl <node ip>:<pot>
```

---

### 二进制方式搭建

#### 环境准备（云服务器无需准备）

- 关闭防火墙

  ```
  systemctl stop firewalld
  systemctl disable firewalld
  ```

- 关闭 selinux

  ```
  sed -i 's/enforcing/disabled/' /etc/selinux/config
  setenforce 0
  ```

- 关闭 swap

  ```
  swapoff -a 
  sed -ri 's/.*swap.*/#&/' /etc/fstab
  ```

- 将桥接的 IPv4 流量传递到 IPtables 链

  ```
  cat > /etc/sysctl.d/k8s.conf << EOF
  net.bridge.bridge-nf-call-ip6tables = 1
  net.bridge.bridge-nf-call-iptables = 1
  EOF
  sysctl --system # 生效
  ```

- 时间同步

  ```
  yum install ntpdate -y
  ntpdate time.windows.com
  ```

#### 准备证书

- 准备 cfssl 证书生成工具

  cfssl 是一个开源证书管理工具，使用json，比openssl更方便，master节点操作

  ```
  wget https://pkg.cfssl.org/R1.2/cfssl_linux-amd64
  wget https://pkg.cfssl.org/R1.2/cfssljson_linux-amd64
  wget https://pkg.cfssl.org/R1.2/cfssl-certinfo_linux-amd64
  chmod +x cfssl_linux-amd64 cfssljson_linux-amd64 cfssl-certinfo_linux-amd64
  
  [root@m1 ~]# mv cfssl_linux-amd64 /usr/local/bin/cfssl
  [root@m1 ~]# mv cfssljson_linux-amd64 /usr/local/bin/cfssljson
  [root@m1 ~]# mv cfssl-certinfo_linux-amd64 /usr/bin/cfssl-certinfo
  
  ```

  



