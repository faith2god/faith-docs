---
date: 2024-01-09
authors: [fuyuanyuan]
description: >
  Prometheus安装
keywords: [prometheus, install]
categories:
  - prometheus
tags:
  - prometheus 
  - install
---

# 二进制包安装

我们可以到 Prometheus 二进制安装包[下载页面](https://prometheus.io/download/)，根据自己的操作系统选择下载对应的安装包。下面我们将以 ubuntu server 作为演示。

## 环境准备

* linux amd64 (ubuntu server)
* prometheus 2.39.1

## 下载 Prometheus Server

创建下载目录,以便安装过后清理掉

```
mkdir ~/Download
cd ~/Download
```

使用 wget 下载 Prometheus 的安装包

```
wget https://github.com/prometheus/prometheus/releases/download/v2.39.1/prometheus-2.39.1.linux-amd64.tar.gz
```

创建 Prometheus 目录，用于存放所有 Prometheus 相关的运行服务

```
mkdir ~/Prometheus
cd ~/Prometheus
```

使用 tar 解压缩 prometheus-2.39.1.linux-amd64.tar.gz

```
tar -xvzf ~/Download/prometheus-2.39.1.linux-amd64.tar.gz
cd prometheus-2.39.1.linux-amd64
```

当解压缩成功后，可以运行 version 检查运行环境是否正常

```
./prometheus --version
```

如果你看到类似输出，表示你已安装成功:

```
prometheus, version 2.39.1 (branch: HEAD, revision: dcd6af9e0d56165c6f5c64ebbc1fae798d24933a)
  build user:       root@273d60c69592
  build date:       20221007-15:57:09
  go version:       go1.19.2
  platform:         linux/amd64
```

## 启动 Prometheus Server

```
./prometheus
```

如果 prometheus 正常启动，你将看到如下信息：

```
ts=2022-10-14T13:51:37.618Z caller=main.go:499 level=info msg="No time or size retention was set so using the default time retention" duration=15d
ts=2022-10-14T13:51:37.621Z caller=main.go:543 level=info msg="Starting Prometheus Server" mode=server version="(version=2.39.1, branch=HEAD, revision=dcd6af9e0d56165c6f5c64ebbc1fae798d24933a)"
ts=2022-10-14T13:51:37.622Z caller=main.go:548 level=info build_context="(go=go1.19.2, user=root@273d60c69592, date=20221007-15:57:09)"
ts=2022-10-14T13:51:37.624Z caller=main.go:549 level=info host_details="(Linux 4.9.184-linuxkit #1 SMP Tue Jul 2 22:58:16 UTC 2019 x86_64 c22888a5ec8e (none))"
ts=2022-10-14T13:51:37.625Z caller=main.go:550 level=info fd_limits="(soft=1048576, hard=1048576)"
ts=2022-10-14T13:51:37.627Z caller=main.go:551 level=info vm_limits="(soft=unlimited, hard=unlimited)"
ts=2022-10-14T13:51:37.644Z caller=web.go:559 level=info component=web msg="Start listening for connections" address=0.0.0.0:9090
```

通过启动日志，可以看到 Prometheus Server 默认端口是 9090。

当 Prometheus 启动后，你可以通过浏览器来访问  `http://IP:9090`，将看到如下页面

![prometheus-graph.png](/docs/images/prometheus-install/prometheus-graph.png)

在默认配置中，我们已经添加了 Prometheus Server 的监控，所以我们现在可以使用 `PromQL` （Prometheus Query Language）来查看，比如：

![prometheus-console.png](/docs/images/prometheus-install/prometheus-console.png)

## 总结

1. 可以看出 Prometheus 二进制安装非常方便，没有依赖，自带查询 web 界面。
2. 在生产环境中，我们可以将 Prometheus 添加到 init 配置里，或者使用 supervisord 作为服务自启动。


# Docker 安装

首先确保你已安装了最新版本的 Docker, 如果没有安装请点击[这里](https://docs.docker.com/engine/installation/)。

下面我将以 Mac 版本的 Docker 作为演示。

## 安装

Docker 镜像地址 [Quay.io](https://quay.io/repository/prometheus/prometheus)

执行命令安装:

```
$ docker run --name prometheus -d -p 9090:9090 quay.io/prometheus/prometheus:v2.39.1
```

如果安装成功你可以访问 `127.0.0.1:9090` 查看到该页面:

![prometheus-graph.png](/docs/images/prometheus-install/prometheus-graph.png)

## Docker 管理 prometheus

运行 docker ps 查看所有服务:

```
CONTAINER ID        IMAGE                           COMMAND                  CREATED             STATUS              PORTS                      NAMES
e9ebc2435387        quay.io/prometheus/prometheus   "/bin/prometheus -..."   26 minutes ago      Up 26 minutes       127.0.0.1:9090->9090/tcp   prometheus
```

运行 `docker start prometheus` 启动服务

运行 `docker stats prometheus` 查看 prometheus 状态

运行 `docker stop prometheus` 停止服务

## Docker 镜像配置参数

通过 Prometheus 的 [Dockerfile](https://github.com/prometheus/prometheus/blob/main/Dockerfile)可以发现，其运行命令为 /bin/prometheus，并包含了4个默认启动参数：

```yaml
ENTRYPOINT [ "/bin/prometheus" ]
CMD        [ "--config.file=/etc/prometheus/prometheus.yml", \
             "--storage.tsdb.path=/prometheus", \
             "--web.console.libraries=/usr/share/prometheus/console_libraries", \
             "--web.console.templates=/usr/share/prometheus/consoles" ]
```

所以，当我们要修改配置 Prometheus 的启动参数，直接覆盖掉默认的即可，例如：

- 修改配置文件和存储目录

```
docker run --name prometheus
    -v=$(pwd)/examples/prometheus.yml:/etc/prometheus/prometheus.yml \
    -v=$(pwd)/examples/tsdb:/prometheus \
    -d -p 9090:9090 \
    quay.io/prometheus/prometheus:v2.39.1
```

此时在 `$(pwd)/examples` 目录下将生产一个 tsdb 目录，存储 Promtheus 时序数据。

- 开启 Lifecycle API

更新启动命令，添加 `--web.enable-lifecycle` 命令参数：

```
docker run --name prometheus \
    -v=$(pwd)/examples/prometheus.yml:/etc/prometheus/prometheus.yml \
    -v=$(pwd)/examples/tsdb:/prometheus \
    -d -p 9090:9090 \
    quay.io/prometheus/prometheus:v2.39.1 \
    --config.file=/etc/prometheus/prometheus.yml \
    --storage.tsdb.path=/prometheus \
    --web.console.libraries=/usr/share/prometheus/console_libraries \
    --web.console.templates=/usr/share/prometheus/consoles \
    --web.enable-lifecycle
```

当重新执行 Docker 命令后并更新 `examples/prometheus.yml` 后，可以通过 `curl -X POST http://localhost:9090/-/reload` 动态加载配置文件。

# kube-prometheus安装
