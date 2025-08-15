---
标题: iSH
aliases: 
tags:
  - 学习/Obsidian
创建时间: 2025-07-18 12:07
修改时间: 2025-08-06 21:54
---
## 一、什么是 iSH

> 一个在 iOS 上模拟 Linux 环境的 app，使用的是 [alpine linux](https://www.alpinelinux.org/)，可以在 iPhone、iPad 上创建并运行 Linux Shell 环境

## 二、安装 ish app

> 从 iOS 应用商店下载 iSH Shell，下载好后打开，你就得到了一个 Linux Shell 环境

整体界面与 Shell 一样，这里重点说一下工具栏：
- Tab：点击后可以自动补全命令
- Ctrl：pc 的 ctrl 健，当你需要执行 `Ctrl + C` 时需要用到
- Esc：pc 的 Esc健
- Arrows：点击后上下左右拖动可以实现 pc 的方向键的功能，常用的是向上拖动调出最新一条历史记录命令

> [!TIP]+ 小技巧
> 操作需要输入一些命令，手机上不太方便，点击工具栏的 Tab，可以自动补全

iSH 中，操作基本上都以 `apk` 命令开头。输出 `apk update` 然后换行即可更新 package。 然后，输入 `uname -a` 来查看操作系统信息：

```shell
localhost:~# uname -a
Linux localhost 4.20.69-ish SUPER AWESOME May 20 2023 23:41:32 i686 Linux
```

## 三、创建 `Obsidian` 仓库

同样地，ios 应用商店下载后打开 obsidian，此时没有任何 vault，首先手动创建一个，比如名称为 obsidian（下文中的所有 obsidian 都可以改为你自己的仓库名称）。此时，你只是创建了一个空的仓库，我们需要让其与 github 同步

## 四、创建 `SSH Key` 并授权给 `GitHub`

我的笔记仓库托管在 github，处于安全考虑，github 禁止使用 http 连接，需要改为 ssh。
打开 iSH app，创建 ssh key，需要安装几个软件：

```shell
apk add git
apk add openssh
```

依次执行上边的命令安装 git、openssh，耐心等待安装完成。
然后，执行以下命令生成 ssh key：

```shell
ssh-keygen -t ed25519 -C "<你的邮箱>"
```

接着，拷贝生成的公钥：

```shell
cat ~/.ssh/id_ed25519.pub
```

登录 github，点击用户头像，在 `setting` -> `SSH and GPG keys` -> `New SSH key` 新建一个授权的key并粘贴刚才拷贝的公钥，保存即可

测试一下在 iSH 中能否正常连接 github：

```shell
localhost:~# ssh -T git@github.com
Hi hankmor! You've successfully authenticated, but GitHub does not provide shell access.
```

## 五、配置 git 环境

```shell
# 打开 iSH，依次执行如下命令
git config --global user.name "你的github用户名"
git config --global user.email "你的github邮箱"
# 此外，还需要添加安全目录，否则git可能出错
git config --global --add safe.directory /mnt/obsidian/obsidian
```

## 六、同步 github 仓库到 iSH

```shell
# 首先，打开 iSH，创建一个 `obsidian` 目录
cd /mnt && mkdir obsidian
# 然后挂载 obsidian app 的文件存储目录到刚才创建的 `obsidian` 目录
mount -t ios . obsidian
# iSH 会弹出一个窗口，在里边选择 `obsidian` 文件夹
# 然后复制 github 仓库的 ssh 地址，进入 `/mnt/obsidian 文件夹
cd /mnt/obsidian/
# 删除下边的 `.obsidian` 文件夹，这是 obsidian 为其仓库创建的，这里先删除，保证这个目录是空的。然后进入上层目录
rm -rf .obsidian
cd ..
# 克隆仓库 ssh
git clone 你的仓库ssh地址 obsidian
# 克隆仓库 http
git clone 你的仓库http地址 obsidian
```

## 七、常用操作

> 这些操作都需要打开 `iSH` app，进入 `/mnt/obsidian` 操作

```shell
# 同步仓库 pc端修改了笔记，ios可以同步仓库
git pull origin main

# 提交更改 ios上修改了笔记，需要提交到 git 仓库
git add .
git commit -m '描述'
git push origin main
```
