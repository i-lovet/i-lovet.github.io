---
标题: Piclist 配合 Openlist 实现图床
aliases: 
tags:
  - 服务/ClawCloud
创建时间: 2025-08-05 19:35
修改时间: 2025-08-06 21:32
---
kuingsmile/piclist:latest

node /usr/local/bin/picgo-server -c /opt/piclist/data/config.json -k H_piclist

配置文件
[config.json](https://ol.1st.ddns-ip.net/d/public/config.json?sign=OdjUaxC77PzHJhtfamZS_z_3w_bl_eb8i0timrcGtjU=:0)

Obsidian 配置
`https://pl.1st.ddns-ip.net/upload?picbed=alistplist&configName=openlist&key=H_piclist`

`https://pl.1st.ddns-ip.net/delete?picbed=alistplist&configName=openlist&key=H_piclist`

初次 Claw 容器 终端设置
```ash
/ # picgo
picgo         picgo-server
/ # picgo-server -c /opt/piclist/data/config.json -k H_piclist
startup
[PicList Server] is listening at 36677
[PicList Server] server is already running at 36677, no need to start again
```
