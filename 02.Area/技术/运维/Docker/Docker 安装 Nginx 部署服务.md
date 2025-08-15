
## 在 Docker 中配置 Nginx 支持 SSL

### 准备工作

- **获取 SSL 证书**：从可信的证书颁发机构（如阿里云、Let's Encrypt）申请证书，并下载 _.crt_ 和 _.key_ 文件。
- **创建目录结构**：在宿主机上创建用于挂载的目录：
```bash
	mkdir -p /path/to/{conf,html,logs,ssl}
```

### 启动 Nginx 容器

- 拉取最新的 Nginx 镜像：
```bash
	docker pull nginx
```

- 启动一个临时容器以获取默认配置文件：
```bash
	docker run --name nginx -d nginx
```

- 将容器内的配置文件复制到宿主机：
```bash
docker cp nginx:/etc/nginx/nginx.conf /path/to/conf
docker cp nginx:/etc/nginx/conf.d /path/to/conf
docker cp nginx:/usr/share/nginx/html /path/to/html
docker cp nginx:/var/log/nginx /path/to/logs
# docker cp temp-nginx:/etc/nginx/ssl /path/to/ssl
```

- 删除临时容器：
```bash
docker rm -f nginx
```

- 启动正式容器：
```bash
docker run --name nginx -p 80:80 -p 443:443 \
-v /path/to/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /path/to/conf/conf.d:/etc/nginx/conf.d \
-v /path/to/ssl:/etc/nginx/ssl \
-v /path/to/html:/usr/share/nginx/html \
-v /path/to/logs:/var/log/nginx \
--privileged=true -d --restart=always nginx

docker run -d --name nginx -p 80:80 -p 443:443 \
-v /usr/local/xuanzun/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /usr/local/xuanzun/nginx/conf/conf.d:/etc/nginx/conf.d \
-v /usr/local/xuanzun/nginx/ssl:/etc/nginx/ssl \
-v /usr/local/xuanzun/nginx/html:/usr/share/nginx/html \
-v /usr/local/xuanzun/nginx/logs:/var/log/nginx \
--privileged=true -d --restart=always nginx
```

### 部署服务

#### manager 部署

>Dockerfile 配置文件
```bash
#基于jdk镜像
FROM openjdk:8
#创建工作目录
WORKDIR /usr/local/xuanzun/project/server/manager
RUN mkdir jar
#将上下文中的jar包添加到工作目录
COPY ./jar/manager-1.0.0.jar /usr/local/xuanzun/project/server/manager/jar
#暴露项目的端口 外部可以访问
EXPOSE 8100
#解决中文乱码
ENV LANG C.UTF-8
#修改时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone
#执行java -jar
ENTRYPOINT ["java","-jar","-server"]
#CMD作为参数放入可变文件
CMD ["-Dspring.profiles.active=prod","./jar/manager-1.0.0.jar"]
```

> nginx 配置文件
```bash
upstream manager-upstream {
    server manager:8100;
}

# http
server {
    listen       80;
    server_name  xforce-xj-manager.timierhouse.com;
    client_max_body_size 200M;

    # 设置读取客户端请求头的超时时间
    client_header_timeout 300s;

    # 设置读取客户端请求主体的超时时间
    client_body_timeout 300s;

    # pad-manager后台服务
    location / {
        proxy_pass http://manager-upstream;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Port $server_port;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_connect_timeout 2s;
        proxy_next_upstream error timeout http_500;
    }
}

# http
server {
    listen       80;
    server_name  xforce-xj-cms.timierhouse.com;
    client_max_body_size 200M;

    # 设置读取客户端请求头的超时时间
    client_header_timeout 300s;

    # 设置读取客户端请求主体的超时时间
    client_body_timeout 300s;


    # 后台前端
    location / {
        root  /usr/share/nginx/html/manager/web/dist;
        index  index.html index.htm ;
        try_files $uri $uri/ /index.html;
    }
}
```

>进入 Dockerfile文件 同级目录
```bash
docker build -t manager .
docker run -dp 8100:8100 --name manager manager
```


#### client 部署

> Dockerfile 配置文件
```bash
#基于jdk镜像
FROM openjdk:8
#创建工作目录
WORKDIR /usr/local/xuanzun/project/server/client
RUN mkdir jar
#将上下文中的jar包添加到工作目录
COPY ./jar/client-1.0.0.jar /usr/local/xuanzun/project/server/client/jar
#暴露项目的端口 外部可以访问
EXPOSE 8110
#解决中文乱码
ENV LANG C.UTF-8
#修改时区
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone
#执行java -jar
ENTRYPOINT ["java","-jar","-server"]
#CMD作为参数放入可变文件
CMD ["-Dspring.profiles.active=prod","./jar/client-1.0.0.jar"]
```

> nginx 配置文件
```bash
upstream client-upstream {
    server client:8110;
}

# http
server {
    listen       80;
    server_name  xforce-xj-client.timierhouse.com;
    client_max_body_size 200M;

    # 设置读取客户端请求头的超时时间
    client_header_timeout 300s;

    # 设置读取客户端请求主体的超时时间
    client_body_timeout 300s;

    # pad后台服务
    location /client {
        proxy_pass http://client-upstream;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Port $server_port;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_connect_timeout 2s;
        proxy_next_upstream error timeout http_500;
    }

    location / {
        alias  /usr/share/nginx/html/client/report/dist;
        index  index.html index.htm ;
        try_files $uri $uri/ /index.html;
    }
}
```

> 进入 Dockerfile文件 同级目录
```bash
docker build -t client .
docker run -dp 8100:8100 --name client client
```

#### 创建并加入网络

```bash
docker network create xxxnetwork

docker network connect xxxnetwork nginx
docker network connect xxxnetwork client
docker network connect xxxnetwork manager
```
