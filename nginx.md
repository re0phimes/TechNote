# 前言

最早接触nginx的时候，觉得就是一个转发有什么难的。直到我自己试了一下。

nginx功能可以大致分为：

- 反向代理
- 负载均衡

这里主要讨论反向代理，也就是location的配置

# 配置项说明

在说location之前，先说一下nginx的配置项，配置项用块的概念区分。默认的配置文件是`nginx.conf`，配置文件中的include语句，可以把`conf.d`这个文件夹或者文件夹下的文件引入。实现份文件配置。

为什么分文件配置？简洁，只要对应的server块就足够了。

## 全局块

- `worker_processes`: 定义 Nginx 进程的数量。
- `error_log`: 定义错误日志的路径和级别。
- `pid`: 定义 Nginx 主进程的 PID 文件路径。

## Events 块

- `use`: 指定使用哪种事件模型（例如 epoll、kqueue）。
- `worker_connections`: 定义每个工作进程的最大连接数。

## Http 块

- `include`: 引入其他配置文件。
- `default_type`: 默认的 MIME 类型。
- `log_format`: 定义日志格式。
- `access_log`: 定义访问日志的路径和格式。

## Server 块

- `listen`: 定义监听的端口和协议。
- `server_name`: 定义服务器的域名。
- `location`: 定义请求的处理规则和路由。
- `index`: 定义默认的首页文件。

## Location 块

- `root`: 定义服务器的根目录。
- `alias`: 定义别名路径。
- `try_files`: 定义请求的文件查找顺序。
- `proxy_pass`: 定义反向代理的地址。

## Fastcgi 配置

- `fastcgi_pass`: 定义 FastCGI 服务器的地址，用于 PHP 等应用。

## Include 指令

- 允许在配置文件中包含其他配置文件。

## Upstream 块

- 定义一组后端服务器，用于负载均衡。
    - `server`: 定义后端服务器的地址和端口。

## Mail 块

- 设置邮件服务器的配置（如果 Nginx 配置为邮件代理）。

## SSL 配置

- 用于配置 HTTPS 服务。
    - `ssl_certificate`: SSL 证书文件路径。
    - `ssl_certificate_key`: SSL 私钥文件路径。

# Location指令

location指令从场景去划分可以理解为：

- 转发到静态文件
    - root
    - alias
    - try_files
        - 后面可以跟多个静态文件，也可以跟变量
- 转发到后端服务
    - proxy_pass
- 重定向
    - rewrite

## 优先级



|     |     |
| --- | --- |
|     |     |


## 匹配方式

location指令不但要匹配uri还要匹配对应的文件，可以尝试创建两个server，试一下是不是真的能匹配到

```text
# case 1
server {
  listen 80;
  server_name localhost;

  location / {
    root /var/www/localhost
  }
}

# case 2
server {
  listen 80;
  server_name localhost;

  location /app {
    root /var/www/localhost
  }
}
```
``
``