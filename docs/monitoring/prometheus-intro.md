---
date: 2024-01-09
authors: [fuyuanyuan]
description: >
  Prometheus
keywords: [prometheus]
categories:
  - prometheus
tags: [prometheus] 
---

# 什么是 Prometheus

[Prometheus](https://prometheus.io) 是由 SoundCloud 开源监控告警解决方案，从 2012 年开始编写代码，再到 2015 年 github 上开源以来，已经吸引了 9k+ 关注，以及很多大公司的使用；2016 年 Prometheus 成为继 k8s 后，第二名 CNCF\([Cloud Native Computing Foundation](https://cncf.io/)\) 成员。

作为新一代开源解决方案，很多理念与 Google SRE 运维之道不谋而合。

## 主要功能

* 多维 [数据模型](https://prometheus.io/docs/concepts/data_model/)（时序由 metric 名字和 k/v 的 labels 构成）。
* 灵活的查询语句（[PromQL](https://prometheus.io/docs/querying/basics/)）。
* 无依赖存储，支持 local 和 remote 不同模型。
* 采用 http 协议，使用 pull 模式，拉取数据，简单易懂。
* 监控目标，可以采用服务发现或静态配置的方式。
* 支持多种统计数据模型，图形化友好。

## 核心组件

* [Prometheus Server](https://github.com/prometheus/prometheus)， 主要用于抓取数据和存储时序数据，另外还提供查询和 Alert Rule 配置管理。
* [client libraries](https://prometheus.io/docs/instrumenting/clientlibs/)，用于对接 Prometheus Server, 可以查询和上报数据。
* [push gateway](https://github.com/prometheus/pushgateway) ，用于批量，短期的监控数据的汇总节点，主要用于业务数据汇报等。
* 各种汇报数据的 [exporters](https://prometheus.io/docs/instrumenting/exporters/) ，例如汇报机器数据的 node\_exporter,  汇报 MongoDB 信息的 [MongoDB exporter](https://github.com/dcu/mongodb_exporter) 等等。
* 用于告警通知管理的 [alertmanager](https://github.com/prometheus/alertmanager) 。

## 基础架构

一图胜千言，先来张官方的架构图

![](https://prometheus.io/assets/architecture.svg)

从这个架构图，也可以看出 Prometheus 的主要模块包含， Server,  Exporters, Pushgateway, PromQL, Alertmanager, WebUI 等。

它大致使用逻辑是这样：

1. Prometheus server 定期从静态配置的 targets 或者服务发现的 targets 拉取数据。
2. 当新拉取的数据大于配置内存缓存区的时候，Prometheus 会将数据持久化到磁盘（如果使用 remote storage 将持久化到云端）。
3. Prometheus 可以配置 rules，然后定时查询数据，当条件触发的时候，会将 alert 推送到配置的 Alertmanager。
4. Alertmanager 收到警告的时候，可以根据配置，聚合，去重，降噪，最后发送警告。
5. 可以使用 API， Prometheus Console 或者 Grafana 查询和聚合数据。

## 注意

* Prometheus 的数据是基于时序的 float64 的值，如果你的数据值有更多类型，无法满足。
* Prometheus 不适合做审计计费，因为它的数据是按一定时间采集的，关注的更多是系统的运行瞬时状态以及趋势，即使有少量数据没有采集也能容忍，但是审计计费需要记录每个请求，并且数据长期存储，这个 Prometheus 无法满足，可能需要采用专门的审计系统。

# 为什么选择 Prometheus

在前言中，简单介绍了我们选择 Prometheus 的理由，以及使用后给我们带来的好处。

在这里主要和其他监控方案对比，方便大家更好的了解 Prometheus。

## Prometheus vs Zabbix

* Zabbix 使用的是 C 和 PHP, Prometheus 使用 Golang, 整体而言 Prometheus 运行速度更快一点。
* Zabbix 属于传统主机监控，主要用于物理主机，交换机，网络等监控，Prometheus 不仅适用主机监控，还适用于 Cloud, SaaS, Openstack，Container 监控。
* Zabbix 在传统主机监控方面，有更丰富的插件。
* Zabbix 可以在 WebGui 中配置很多内容，但是 Prometheus 需要手动修改文件配置。

## Prometheus vs Graphite

* [Graphite](http://graphite.readthedocs.io/en/latest/overview.html) 功能较少，它专注于两件事，存储时序数据，
可视化数据，其他功能需要安装相关插件，而 Prometheus 属于一站式，提供告警和趋势分析的常见功能，它提供更强的数据存储和查询能力。
* 在水平扩展方案以及数据存储周期上，Graphite 做的更好。

## Prometheus vs InfluxDB

* [InfluxDB](https://www.influxdata.com/) 是一个开源的时序数据库，主要用于存储数据，如果想搭建监控告警系统，
需要依赖其他系统。
* InfluxDB 在存储水平扩展以及高可用方面做的更好, 毕竟核心是数据库。

## Prometheus vs OpenTSDB

* [OpenTSDB](http://opentsdb.net/) 是一个分布式时序数据库，它依赖 Hadoop 和 HBase，能存储更长久数据，
如果你系统已经运行了 Hadoop 和 HBase, 它是个不错的选择。
* 如果想搭建监控告警系统，OpenTSDB 需要依赖其他系统。

## Prometheus vs Nagios

* [Nagios](https://www.nagios.org/) 数据不支持自定义 Labels, 不支持查询，告警也不支持去噪，分组, 没有数据存储，如果想查询历史状态，需要安装插件。
* Nagios 是上世纪 90 年代的监控系统，比较适合小集群或静态系统的监控，显然 Nagios 太古老了，很多特性都没有，相比之下Prometheus 要优秀很多。

## Prometheus vs Sensu

* [Sensu](https://sensuapp.org/) 广义上讲是 Nagios 的升级版本，它解决了很多 Nagios 的问题，如果你对 Nagios 很熟悉，使用 Sensu 是个不错的选择。
* Sensu 依赖 RabbitMQ 和 Redis，数据存储上扩展性更好。

## 总结

* Prometheus 属于一站式监控告警平台，依赖少，功能齐全。
* Prometheus 支持对云或容器的监控，其他系统主要对主机监控。
* Prometheus 数据查询语句表现力更强大，内置更强大的统计函数。
* Prometheus 在数据存储扩展性以及持久性上没有 InfluxDB，OpenTSDB，Sensu 好。