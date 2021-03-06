---
title: 一文记录国内各种镜像源的使用方法
date: 2021-02-28 09:47:32
tags:
---

#### 1. Ubuntu apt 源

Ubuntu 的软件源配置文件是 `/etc/apt/sources.list`。在修改该文件前，先做一份备份：

```shell
sudo cp /etc/apt/sources.list /etc/apt/source.list.bak
```

根据你ubuntu（例如：18.04.LTS）的版本，到[Ubuntu 镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/)复制版本对应配置信息。

然后打开`/etc/apt/sources.list`，将所有的内容替换为你复制的内容并保存。我使用的版本是`18.04LTS`版本，内容如下：

```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

#### 2. nvm node npm 源

在 Ubuntu 下，安装 nvm 方式为：

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
```

或者

```shell
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
```

安装完成后，打开一个新的终端，输入`nvm`并回车即可验证是否安装成功。

#### 3. npm package 淘宝源

```shell
npm config set registry https://registry.npm.taobao.org --global 
```

#### 4. Pycharm pip 源

```
https://pypi.tuna.tsinghua.edu.cn/simple
```

#### 5. Docker image 源

