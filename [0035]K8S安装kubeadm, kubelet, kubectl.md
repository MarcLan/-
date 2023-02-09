## K8S安装kubeadm, kubelet, kubectl

[TOC]

### 准备

- linux主机
- RAM> 2
- CPU Core> 2
- 集群中所有机器网络互通

### 检查端口

```
nc 127.0.0.1 6443
```

### 安装kubeadm kubelet kubectl

- **kubeadm**: 初始化集群的指令

- **kubelet**: 在集群中的每个节点上启动Pod和容器

- **kubectl**: 用来与集群通信的命令行工具

- 基于Red Hat的发行版

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
  
  # 将 SELinux 设置为 permissive 模式（相当于将其禁用）
  sudo setenforce 0
  sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
  
  sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
  
  sudo systemctl enable --now kubelet
  ```

- 请注意

  - 通过运行命令 `setenforce 0` 和 `sed ...`将SELinux设置为permissive模式可以有效地将其禁用。这是允许容器访问主机文件系统所必需的，而这些操作事为了例如 Pod 网络工作正常

  - 如果你知道如何配置 SELinux 则可以将其保持启用状态，但可能需要设定 kubeadm 不支持的部分配置

  - 如果由于该 Red Hat 的发行版无法解析 `basearch` 导致获取 `baseurl` 失败，请将 `\$basearch` 替换为你计算机的架构。 输入 `uname -m` 以查看该值。 例如，`x86_64` 的 `baseurl` URL 可以是：`https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64`。