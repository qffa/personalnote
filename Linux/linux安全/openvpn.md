#OpenVPN

## 安装

2台机器：1台安装openvpn，1台作为CA。两个角色也可以装在一台机器上（不推荐）
系统：CentOS 7，关闭SELINUX, Firewalld, NetworkManager

**OpenVPN Server**

```
$ yum install openvpn
$ yum install openvpn-auth-ldap #如果需要LDAP认证
```

**CA Server**

```
$ yum install easy-rsa
```


## 准备证书

### 创建CA

如果企业有CA，不用执行此步骤

on CA Server
```
$ rpm -ql easy-rsa  # 找到easy-rsa的安装目录，这里是/usr/share/easy-rsa/3

$ cd /usr/share/easy-rsa/3
$ mkdir lab.local   # 创建一个目录，用于存放一套ca的pki相关文件
$ ../easy-rsa init-pki  # 初始化pki目录
$ ../easy-rsa --dn-mode=org build-ca  # 创建CA
```

*注意：ca私钥的passcode一定要记好了*

### 申请openvpn server的证书

如果企业有CA，使用企业证书申请流程。此处使用easy-rsa

**创建CSR**

on OpenVPN server

openssl方式

```
$ openssl genrsa -out openvpn01.key 2048
$ openssl req -new -key openvpn01.key -out openvpn01.csr
```

[可选方式]如果openvpn server也安装了easy-rsa也可以使用easy-rsa生成csr文件
```
$ ../easyrsa --dn-mode=org gen-req openvpn01 nopass
# req文件在这个目录下/usr/share/easy-rsa/3/lab.local/pki/reqs/
```


**签发证书**

on CA server

将CSR文件拷贝到CA服务器上，在CA上执行签发操作
```
$ ../easyrsa import-req pki/reqs/openvpn01.req openvpn01
$ ../easyrsa sign-req server openvpn01

# 证书在这：/usr/share/easy-rsa/3/lab.local/pki/issued/
```

### 申请客户端证书

**生成CSR**

根据客户端平台不同而不同

**签发**

企业证书签发流程或者easy-rsa

*或者在服务器上生成好证书和key，然后拷贝到客户端。这样不正规、不安全*


### 创建其他文件

On OpenVPN Server

**DH 参数文件**

```
$ ../easyrsa gen-dh
#or
$ openssl dhparam -out dh2048.pem 2048

```

**secret file**

```
$ openvpn --genkey --secret ta.key

```

## 将证书和相关文件拷贝到openvpn配置路径

### OpenVPN Server

文件位置：/etc/openvpn/server/

所需文件

- ca.crt
- openvpn01.crt
- openvpn01.key
- dh2048.pem
- ta.key

*为了安全，权限给600。最起码key的权限给成600*

### Client

文件位置（参考）：C:\Users\qff\OpenVPN\config\

所需文件

- ca.crt
- client01.crt
- client01.key
- ta.key




## 配置

基本配置，只确保vpn能通

### Server

/etc/openvpn/server/server.conf

```
port 1194
proto udp
dev tun
ca /etc/openvpn/server/ca.crt
cert /etc/openvpn/server/openvpn02.crt
key /etc/openvpn/server/openvpn02.key  # This file should be kept secret
dh /etc/openvpn/server/dh.pem
server 10.8.0.0 255.255.255.0
ifconfig-pool-persist ipp.txt
push "redirect-gateway def1 bypass-dhcp" # 所有流量走vpn
push "dhcp-option DNS 114.114.114.114"
client-to-client
keepalive 10 120
tls-auth /etc/openvpn/server/ta.key 0 # This file is secret
cipher AES-256-CBC
persist-key
persist-tun
status /var/log/openvpn-status.log
log         /var/log/openvpn.log
log-append  /var/log/openvpn.log
verb 3
explicit-exit-notify 1
```

### Client

C:\Users\qff\OpenVPN\config\client.ovpn

```
client
dev tun
proto udp
remote 192.168.168.10 1194
resolv-retry infinite
nobind
persist-key
persist-tun
ca ca.crt
cert client.crt
key client.key
remote-cert-tls server
tls-auth ta.key 1
cipher AES-256-CBC
verb 3
```

## 启动服务器

```
$ systemctl start openvpn-server@server
# or
$ openvpn --daemon --config /etc/openvpn/server/server.conf
```

## 配置内核参数和防火墙

```
$ vim /etc/sysctl.conf
net.ipv4.ip_forward = 1
$ sysctl -p
$ iptables -A INPUT -p tcp -m tcp --dport 1194 -j ACCEPT
$ iptables -A INPUT -p udp --dport 1194 -j ACCEPT
$ iptables -t nat -A POSTROUTING -s 10.8.0.0/24  -j MASQUERADE
```

## 启动客户端连接vpn server

windows下运行openvpn client GUI，任务栏图标右键点击connect
