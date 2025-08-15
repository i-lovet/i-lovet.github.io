---
标题: Docker 设置阿里云镜像加速器
aliases: 
tags:
  - 技术/Docker
创建时间: 2025-08-09 14:37
修改时间: 2025-08-09 14:41
---



Docker 默认情况下是从官方 Docker Hub 下载镜像，但是由于 Docker Hub 服务器在国外的缘故，国内用户在下载镜像的时候，会非常慢。

想要解决这个问题，我们可以配置国内的镜像加速器，如阿里云加速器、DaoCloud、网易加速器、百度云加速器等等。

## 登录阿里云，进入控制台

进入到阿里云控制台后，搜索关键字【**镜像服务**】, 点击【**容器镜像服务**】：

## 复制镜像服务器地址

点击左边菜单栏【**镜像加速器**】，复制加速器地址：

## 开始配置镜像加速器

修改 daemon 配置文件 `/etc/docker/daemon.json` 来使用加速器
```bash
sudo mkdir -p /etc/docker 
sudo tee /etc/docker/daemon.json <<-'EOF' {"registry-mirrors": ["https://xxx.mirror.aliyuncs.com"] }

或者使用 vi 编辑器 写入

```
