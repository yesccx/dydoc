# 简介 dydoc

docker + ydoc(运行环境)，整个环境由以下三个服务主成：
- mysql5.6
- php7.2
- nginx1.14.2

> 每个服务容器可以独立进入操作


# 开始使用

## 1. 基本准备

确保系统中已安装了`docker` 及 `docker-compose`，如果需要在仓库中拉取配置文件，还需要安装 `git`。

```
git clone https://github.com/yesccx/dydoc.git 或 手动下载压缩文件解压
```

## 2. 相关配置

- 复制并编辑根目录下的 `.env.example` 文件，配置 `DC_PATH`、`APP_PATH` 与 `APP_API_PATH` 项，分别为 `dydoc`、`ydoc` 与 `ydoc-api` 在本机中的所在目录。
- 配置 `nginx` 虚拟主机配置项 `conf/nginx/conf.d/ydoc.conf`，通常只需修改 `server_name`，同时需要修改本机的 `hosts`，加入一条域名指向。


## 3. 启动服务

进入 `dydoc` 目录，执行 `docker-compose up -d --build`，初次使用时可能需要等待一段时间，如果超时，建议更换 `docker源`。

待完成后服务会自动启动，默认的http访问地址为`www.y.com:8181`，可以手动在`conf/nginx/conf.d/ydoc.conf` 中配置 `server_name`，与在`.env`环境变量中配置的 `HTTP_PORT` 端口号。

> 之后除非更改了配置项时需要执行 `docker-compose build` 重新构建，否则每次只需要 `docker-compose start|stop|restart` 管理服务即可。

> 如果非`localhost`，需要在本机添加一条hosts指向域名


# 配置项

可配置内容有根目录下的`conf`中的配置文件，包括 `php`、`php-fpm`、`mysql`、`nginx`，还有根目录下的 `.env` 环境配置文件。

### 服务conf

与常规的`lamp` 相关配置方式一致


### .env环境配置文件

|  KEY | 说明  |
| ------------ | ------------ |
| APP_PATH |  ydoc项目所在目录 |
| APP_API_PATH |  ydoc-api项目所在目录 |
| DC_PATH  |  dydoc所在目录 |
| MYSQL_PORT  |  mysql服务暴露的端口，可以默认 |
| HTTP_PORT  |  nginx服务暴露的端口，可以默认 |

> 注意这里的目录，如果是windows系统，可能docker容器会访问不到自定义的目录，需要先在虚拟机中加入共享



# 常见问题

### 开启、关闭、重启服务

在 `dydoc` 目录下 执行 `docker-compose start|stop|restart` 命令

### 查看日志文件

日志文件存放在根目录下的 `log` 目录下。

### 更新配置文件

如果更新了配置文件，重启服务未生效时，可以先执行 `docker-compose build` 重新构建服务后，再启动服务。

### 独立进入服务

启动后会默认开启3个名称分别为`dy_mysql` `dy_nginx` `dy_php` 的独立服务，利用`docker exec` 命令进入，如：

```
docker exec -it dy_nginx /bin/bash
```

### 更换Docker源

> 待补充