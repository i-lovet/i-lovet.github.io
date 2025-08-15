---
标题: Worker & Pages 自建短链服务
aliases: 
tags:
  - 服务/Cloudflare
创建时间: 2025-08-02 11:01
修改时间: 2025-08-06 21:40
---
> [!note]- 账号申请
> 申请 [`Cloudflare`](https://www.cloudflare.com/zh-cn/) 账号并登录

> [!tip]- 请求地址 及 密码
> - [`shorten.1st.ddns-ip.net`](https://shorten.1st.ddns-ip.net/)
> - [`sink-url.pages.dev`](https://sink-url.pages.dev/)
> - `H_kvSink`

## 一、Url-Shorten-Worker

> 基于 Cloudflare Worker 的短链工具

> [!note]- 参考
> - [`源码 GitHub`](https://github.com/crazypeace/Url-Shorten-Worker)
> - [`演示站`](https://urlsrv.crazypeace.workers.dev/bodongshouqulveweifengci)

### 1.1 创建一个 `Worker` ，以<font color=#B91E8C> KVData </font>为例

![20250802104920.png](https://ol.1st.ddns-ip.net/d/public/20250806/20XZAX.webp?sign=JXwYdA9NqvAVFXM23l-IneVoTEKe5Bkr-1LnWO9odtU=:0)

![20250802104920-1.png](https://ol.1st.ddns-ip.net/d/public/20250806/e10heu.webp?sign=hfQw5LrRaNsNc6iCh3EWS25cjn5FF3Io7LRwPLuc_iQ=:0)

### 1.2 添加并绑定命名空间

![20250802104920-2.png](https://ol.1st.ddns-ip.net/d/public/20250806/MMH74r.webp?sign=DY_Na152YgPgQXq72XMoGVNgAaJn-3269VV2tyJEErM=:0)

![20250802104920-3.png](https://ol.1st.ddns-ip.net/d/public/20250806/b4KyOr.webp?sign=EVl1hpj-IoyC8uPBkE4DoO_O5CmAMT18BP9UaQ2UoL8=:0)

![20250802104920-4.png](https://ol.1st.ddns-ip.net/d/public/20250806/9OqmGB.webp?sign=D-NuJO1hycWjavrwkZDl_iAq58bQIHtfLHI5ShUDWDY=:0)

### 1.3 编辑代码并部署
 
- Fork一份代码
- 替换 [`worker.js`](https://github.com/i-lovet/Url-Shorten-Worker/blob/main/worker.js) 中相关GitHub用户名
- 复制内容到创建好的`Worker`代码空间下，部署

### 1.4 添加自定义域名

1) 通过添加`Workers`路由
2) 通过`DNS`创建`CNAME`转发


## 二、Sink

> [!note]- 参考
> - [`源码 GitHub`](https://github.com/ccbikai/Sink)
> - [`html.zone`](https://html.zone/)
> - [`搭建Sink短链接`](https://juejin.cn/post/7494157121388298249)

### 2.1 部署

-  Fork 一份代码
- `Workers`填入 Github 仓库部署
- `Pages`选择 Github 仓库部署

|         变量名          |                     值                      |
| :------------------: | :----------------------------------------: |
| `NUXT_CF_ACCOUNT_ID` |     `53605409352830c2e5db236da11ebe22`     |
| `NUXT_CF_API_TOKEN`  | `HxcG9CHStEYqP2JNOnE0q1ij7eg04TAJCmatFqcV` |
|  `NUXT_SITE_TOKEN`   |                 `H_kvSink`                 |

### 2.2 绑定

|        类型        |    名称    |  值  |
| :--------------: | :------: | :-: |
|     KV 命名空间      | `SinkKV` |     |
| Analytics Engine | `SinkAE` |     |
|    Workers AI    | `SinkAI` |     |

### 2.3 运行时

兼容性标志         `nodejs_compat`
