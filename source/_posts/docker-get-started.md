---
title: Docker 入门
date: 2021-02-27 11:10:07
tags:
---

本文是 Docker 学习笔记。

#### Docker 实用技巧1：安装Docker
本人使用 linux 发行版之一的 ubuntu 来学习 Docker，这里只记录 ubuntu 下的安装方法。
一行命令即可搞定：

```sh
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

其他系统的 Docker 安装方法可点击[这里](https://www.runoob.com/docker/ubuntu-docker-install.html)查看。

#### Docker 使用技巧2：镜像加速

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

#### Docker 使用技巧3：安装jenkins

一行命令搞定：

```shell
docker run -d --name jenkins -p 8888:8080 -p 50000:50000 -v /home/workspace/jenkins_home:/var/jenkins_home -u 0 jenkins/jenkins
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

