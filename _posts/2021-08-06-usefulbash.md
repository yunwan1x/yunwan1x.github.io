---
layout: post
title: 有用的命令行
categories: github
---

一些比较有用的bash命令。 记录如下，方便查找。

```bash
#linux 三剑客

# apt install mc ，midnight commander
mc 
# xargs [链接](https://www.ruanyifeng.com/blog/2019/08/xargs-tutorial.html)

xargs 
# 删除所有的docker images
docker images |awk '{print $1":"$2}'|sed -n '2,$p'|xargs docker image rm
```

# python 虚拟环境以及requirement.txt

## 虚拟环境

```bash
# 建立快捷方式
ln -s ${sourcefile} $linkname
ln -sf /usr/local/bin/python3 /usr/bin/python 
echo alias python=/usr/local/bin/python3 >> ~/.zshrc

# 建立虚拟环境
mkdir $dir
cd $dir
python -m venv .
source ./bin/activate

deactive #退出环境
```

## requirements.txt

```bash
# 生成requirements.txt文件
pip freeze > requirements.txt

# 安装requirements.txt依赖
pip install -r requirements.txt
```

# nginx ssl证书相关

```bash
# 设置chrome信任本地localhost证书
chrome://flags/#allow-insecure-localhost

#!/bin/bash
openssl genrsa -des3 -out ssl.key 2048
mv ssl.key 1.key
openssl rsa -in 1.key -out ssl.key
rm 1.key
openssl req -new -key ssl.key -out ssl.csr
openssl x509 -req -days 3650 -in ssl.csr -signkey ssl.key -out ssl.crt
```

# io相关

```bash
# 查看进程pid打开的tcp4端口
lsof -i 4 -a  -p $pid
```

# k8s的网络

1. 网络调试打通，参考[kube-openvpn7](https://github.com/pieterlange/kube-openvpn)，[k8s-openvpn](https://www.1nth.com/post/k8s-openvpn/) ， [打通网络的n种方法](https://zhuanlan.zhihu.com/p/187548589) 。
2. 调试容器工具。[kubectl-debug](https://cloud.tencent.com/developer/article/1548622) ，[kubespy](https://github.com/huazhihao/kubespy) ,
3. vscode的bridge to kubernetes

# 网络相关

## dns地址相关

```bash
# 查询dns
nslookup [-option] [name | -] [server] 
host name
dig @server name [type: A]
```

## 代理转发

* ssh ，可参考[ssh权威指南](https://www.google.com/url?sa=t&source=web&rct=j&url=https://b.nop.pw/wp-content/uploads/2014/09/A2063137.pdf&ved=2ahUKEwjcufr93IPzAhVLnJ4KHU5kCIIQFnoECBAQAQ&usg=AOvVaw0vuQA2_xo452L2aMPVtbVr&cshid=1631803304083)

```bash
# 本地端口转发，转发本地的2000端口到
#参考 https://wangdoc.com/ssh/port-forwarding.html
remotehost的3000端口，-f 后台，-N不需要接命令
ssh -N -f -L localhost:2000:remote:3000 root@tunnelhost
```

* socat网络瑞士军刀。nmap端口扫描。 

## ip路由相关

对于有多个网口或者虚拟网口的机器，ip route会非常有用。

```bash
#查看所有的网口
ip link 

#查看所有的ip地址
ip address 

#查看邻居表
ip neigh 

# 添加到dst的路由
ip route add  10.100.0.0/24  via 192.168.56.3

# 可用于vpn访问相关，utun2为vpn虚拟网口设备名
ip route add 47.99.0.0/16 dev utun2

# 查询到某个ip地址的路由
ip route get $dst
```

## vpn

* wireguard

# docker

```bash
docker save <镜像名> | bzip2 | pv | ssh <用户名>@<主机名> 'cat | docker load'
```

