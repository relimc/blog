---
title: Docker 入门
date: 2021-02-27 11:10:07
tags:
---

本文是 Docker 学习笔记。以下所有操作均在 Ubuntu 下执行。

#### Docker 实用技巧1：安装Docker
本人使用 linux 发行版之一的 ubuntu 来学习 Docker，这里只记录 ubuntu 下的安装方法。
一行命令即可搞定：

```sh
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

其他系统的 Docker 安装方法可点击[这里](https://www.runoob.com/docker/ubuntu-docker-install.html)查看。

#### Docker 实用技巧2：镜像加速

为了避免后期使用 Docker 出现镜像下载速度过慢的问题，我们可以使用阿里云的容器镜像服务。前提是你已经有了阿里云的账号。戳这里的[文档](https://cr.console.aliyun.com/cn-shenzhen/instances/mirrors)即可知道加速方法。文档内针对 ubuntu 的配置方法如下：

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["你的加速器地址"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

#### Docker 实用技巧3：安装jenkins

一行命令搞定：

```shell
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v /home/workspace/jenkins_home:/var/jenkins_home --user=root jenkins/jenkins
sudo docker run --name jenkins -p 8080:8080 -p 50000:50000 -v /volume2/data/jenkins_home:/var/jenkins_home --user=root --restart=always -v /var/run/docker.sock:/var/run/docker.sock jenkinsci/blueocean
```

加速处理，切换为清华源

```shell
cd /home/workspace/jenkins_home
vim hudson.model.UpdateCenter.xml
```

修改后的内容如下

```html
<?xml version='1.1' encoding='UTF-8'?>
<sites>
  <site>
    <id>default</id>
    <url>https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json</url>
  </site>
</sites>
```

查看用于首次登录的密码

```shell
cat /home/workspace/jenkins_home/secrets/initialAdminPassword
```

#### Docker 实用技巧4：Jenkins远程访问Docker

一般情况下，我们只能本地访问 Docker 守护进程，要想远程访问 Docker，需要做些配置。

```shell
vim /lib/systemd/system/docker.service
```

在文件中找到**ExecStart**所在的那行，并添加`-H tcp://0.0.0.0:2375`。

```
# 原来
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
# 修改后
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H fd:// --containerd=/run/containerd/containerd.sock
```

然后重启

```shell
systemctl daemon-reload
systemctl restart docker
```

检查下本地访问是否正常：

```shell
docker -H 127.0.0.1:2375 images
```

```shell
firewall-cmd --zone=public --add-port=2375/tcp --permanent
firewall-cmd --reload
```

Docker 侧已开放远程访问的权限，我们在 Jenkins 侧看是否能够访问。

[Docker开启远程安全访问](https://www.cnblogs.com/niceyoo/p/13270224.html)



















































