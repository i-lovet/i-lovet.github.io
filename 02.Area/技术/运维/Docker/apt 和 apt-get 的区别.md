---
标题: apt 和 apt-get 的区别
aliases: 
tags:
  - 技术/服务器
创建时间: 2025-08-09 13:58
修改时间: 2025-08-09 14:47
---
> 为什么 Debian 系 Linux 发行版同时拥有`apt`和`apt-get`这两个雷同的命令？

虽然`apt`和`apt-get`都可用于安装、删除和管理软件包，但存在一些关键区别：

- **用户体验**：`apt`的输出更加现代友好，彩色输出和进度条让操作过程一目了然。而`apt-get`则保持着传统的纯文本输出，略显单调。
- **功能差异**：`apt`不仅包含了`apt-get`的所有功能，还新增了`list`、 `search`等实用命令，功能更加全面。
- **脚本编写**：`apt-get`输出格式稳定，更适合用于编写脚本，实现自动化管理。

简单来说，`apt`更适合日常使用，而`apt-get`更受脚本开发者青睐。下表列出了两者的命令和功能说明，选择使用哪一个，取决于你的具体需求和个人喜好。

| apt 命令           | apt-get 命令           | 命令的功能           |
| ---------------- | -------------------- | --------------- |
| apt install      | apt-get install      | 安装软件包           |
| apt remove       | apt-get remove       | 移除软件包           |
| apt purge        | apt-get purge        | 移除软件包及配置文件      |
| apt update       | apt-get update       | 刷新存储库索引         |
| apt upgrade      | apt-get upgrade      | 升级所有可升级的软件包     |
| apt autoremove   | apt-get autoremove   | 自动删除不需要的包       |
| apt full-upgrade | apt-get dist-upgrade | 在升级软件包时自动处理依赖关系 |
| apt search       | apt-cache search     | 搜索应用程序          |
| apt show         | apt-cache show       | 显示包的详细信息        |

当然，`apt`还有一些自己的命令：

|新的 apt 命令|命令的功能|
|---|---|
|apt list|列出包含条件的包（已安装、可升级等）|
|apt edit-sources|编辑源列表|

总而言之：
- `dpkg`是幕后英雄，负责实际的安装工作。
- `apt-get`提供了完整的`dpkg`接口，功能强大但略显繁琐。
- `apt`是更友好、更易用的`apt-get`版本，功能略有精简。

`apt`和`apt-get`不仅仅是 `dpkg`的接口，还能完`dpkg` 无法做到的事情，比如从软件库获取文件。选择使用哪一个，取决于你的具体需求和使用场景。
