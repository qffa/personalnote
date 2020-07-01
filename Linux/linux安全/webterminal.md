# webterminal安装

## 安装方式

使用docker方式运行

## 安装步骤

### 安装docker

**安装依赖**

```
$ yum install -y yum-utils
$ yum install -y lvm2
$ yum install device-mapper-persistent-data
```

**添加docker的yum源**

```
$ yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

**安装docker-ce**

```
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

**添加docker仓库国内镜像**

```
$ vim /etc/docker/daemon.json

{
  "registry-mirrors": ["https://n6syp70m.mirror.aliyuncs.com"]
}
```

**启动docker**

```
$  systemctl start docker
```

### 使用docker运行webterminal

**拉取webterminal镜像**
```
$ docker pull webterminal/webterminal
$ docker image ls
```

**创建webterminal容器**

```
$ docker container create --name webterminal01 -it -p 80:80 -p 2100:2100 webterminal/webterminal
```

**管理容器**

```
$ docker container ls -a
$ docker container start webterminal01 # 访问之前需要启动容器
$ docker container stop webterminal01
$ docker container restart webterminal01
```

### 访问webterminal

http://server_ip

- username: admin
- password: password!23456
