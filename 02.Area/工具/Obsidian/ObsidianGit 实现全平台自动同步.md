---
标题: ObsidianGit 实现全平台自动同步
aliases: 
tags:
  - 学习/Obsidian
创建时间: 2025-07-18 12:07
修改时间: 2025-08-06 21:52
---
> 实现同步的设备有： Mac、Windows、Android、IOS 等四个平台。主要借助的是 Git 的能力，刚好可以实现版本的管理。

## 1、Mac 端配置

### 1.1 新建仓库

1. 新建一个仓库，这里我以  obsidian  创建一个仓库，并在 Obsidian 中打开。
2. 安装插件

> [!tip] 提示
> 在设置中找到第三方插件，关闭安全模式，然后就可以安装插件了

### 1.2 创建 Git 仓库

> 在 Github 上创建同名仓库，当然可以使用自己搭建的 Git 服务器以及直接使用国内的 Gitee。注意我这里选择的是私有的库。

进入到本地仓库  obsidian  的文件目录下，执行以下命令：
```shell
git init  // 初始化 git 项目
git remote add origin https://gitee.com/i-lovet/obsidian.git // 添加远程仓库
```

添加 gitignore 配置，防止一些配置文件被提交上去了。（主要是不同设备的配置导致冲突，会影响自动同步）

```shell 
# 忽略 Obsidian 的工作区配置文件（动态变化）
.obsidian/workspace*
# 忽略 Obsidian 的回收站文件夹
.obsidian/.trash/
# 忽略所有日志文件
*.log
# 忽略临时文件或系统生成的文件
DS_Store
Thumbs.db 
```

## 2、IOS 端配置

> 在了解过很多 IOS 端的配置后，看到很多都是使用了 Working Copy，使用 Working Copy 来实现手动同步远程 Git 仓库。 但是这种方案有两个缺点：

1. Working Copy 关键功能是收费的；
2. 需要手动同步。因此还是想借助 Obsidian 的 Git 插件能力来实现自动同步。

### 2.1 创建仓库

在 IOS 端要先创建一个仓库，这里的名称还是叫 obsidian 

### 2.2 配置 iSH

[iSH](iSH.md) 是一个IOS 端的 Linux Shell 工具，我们用它来拉取 git 的远程仓库

```Shell
apk update  // 更新软件包
apk install git   // 安装 git
```
### 2.3 挂载目录

在 IOS 端找到 Obsidian 的目录，在 ISH 中挂载到相应的目录

```Shell
mount -t ios obsidian /mnt
// 此时会让你选择目录，选择 obsidian 目录就好。
cd /mnt/obsidian  
// 查询挂载
mount | grep /mnt
// 取消挂载
umount /mnt
```

### 2.4 配置

#### 2.4.1 配置 Git

```Shell
git config --global user.name "wolf"
git config --global user.email "i.coding.wolf@qq.com"
```

#### 2.4.2 配置远程仓库

```Shell
git init   // 初始化 git​  
git branch -M main  // 调整分支为 main
git remote add origin https://gitee.com/i-lovet/obsidian.git  // 添加远程仓库​  
git pull origin main // 拉取远程代码
git config --global --add safe.directory /mnt/obsidian  // 添加安全目录
```

> [!caution]+ 注意
> 这里没有使用 SSH，这里使用的是 Https，使用的是账号 + Token 的认证方式。[创建 Token](https://gitee.com/profile/personal_access_tokens)  创建一个 Token

#### 2.4.3 配置 Git 插件

打开 Obsidian 此时已经存在远程的文件了，在手机端也需要安装 Git 插件，同样在第三方插件中关闭安全模式后可以安装

打开 Git 插件的配置栏，自动同步的配置保持和 Mac 端的一致，需要配置 Git 的栏目，因为自动同步其实是靠 Git 插件来同步的。依次把以下的配置完成同步即可

在 Git 插件的配置中，找到 Username、Token、name、email，填入正确的值即可

## 3、Android 端配置

> 在 IOS 端有 Working Copy，Android 也有类似的软件叫做 MGit，它是 Android 上的 Git 客户端，可以实现手动同步，但是为了和 IOS 端保持一致，还是选择了 Termux，Termux 是 Android 上的 Linux Shell 工具，通过它来安装 Git

### 3.1 安装 Termux

> 下载安装之后，打开Termux，在 Termux 中申请本机存储访问权限，之后使Termux可访问本机存储

```shell
termux-setup-storage
// 下载依赖
pkg update
pkg install git
```

### 3.2 配置目录

> 创建存放目录，Android 中是可以直接打开一个目录作为仓库的，可以自定义目录，这里我选择的是 Documents/Obsidian/obsidian

通过cd命令，进入本机 Obsidian 文件夹的 obsidian 文件夹内。不同手机型号地址位置可能有所不同，例如：

```shell
cd ~/storage/documents/Obsidian/obsidian
```

### 3.3 配置Git
#### 3.3.1 配置 Git

```Shell
// 配置 Git
git config --global user.name "i-lovet"
git config --global user.email "i.coding.wolf@qq.com"
// 配置远程仓库
git init   // 初始化 git​  
git branch -M main  // 调整分支为 main
git remote add origin https://gitee.com/i-lovet/obsidian.git  // 添加远程仓库​  
git pull origin main // 拉取远程代码
git config --global --add safe.directory /storage/emulated/0/Documents/Obsidian/obsidian  // 添加安全目录
```

#### 3.3.2 配置 Git 插件

> 在 Obsidian 打开刚才拉取的远程仓库，在 Git 插件的配置中，找到 Username、Token、name、email，填入正确的值即可
