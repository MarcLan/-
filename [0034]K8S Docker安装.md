## CentOS安装Docker

[TOC]

### 删除老的docker

```
 sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 设置新的存储库

```
sudo yum install -y yum-utils
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

### 安装docker

- 安装

  ```
  sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
  ```

- 查看版本

  ```
  yum list docker-ce --showduplicates | sort -r
  
  docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
  docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
  docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
  docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable
  ```

- 安装版本

  ```
  yum install docker-ce-20.10.10 docker-ce-cli-20.10.10 containerd.io docker-compose-plugin
  ```

- 启动Docker

  ```
  sudo systemctl start docker
  ```

- 验证

  ```
  sudo docker run hello-world
  ```

### 链接

 https://docs.docker.com/engine/install/centos/#install-using-the-repository





