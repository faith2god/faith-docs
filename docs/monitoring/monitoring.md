# 监控概览
从程序设计上来看，监控可分为

- 应用程序监控（APM）
- 中间件监控
   - 类型
      - 消息中间件，RabbitMQ,Kafka
      - Web服务中间件，Tomcat,Jetty
      - 缓存中间件，Redis,Memcached
      - 数据库中间件，Mysql,PostgreSQL
   - 监控方式
      - 不同中间件监控方式并不相同
      - 一般采用agent
      - 对于Prometheus，每种都有一种对应的exporter
- 基础资源监控
   - 网络监控——解决方案：天旦、nCompass，SolarWinds, Nagios（已开源）
      - 数据中心内 网络流量
      - 网络拓扑发现及网络设备
         - 对数据中心内的多种网络设备进行监控，路由器，防火墙，交换机等硬件
         - 网络设备
            - 流量镜像
               - 配置交换机上的镜像端口
               - 虚拟设备——tcpdump等工具获取网络数据包，根据源和目的IP汇聚网络拓扑结构
            - 通过SNMP（Simple Network Management Protocol 简单网络管理协议）等协议收集数据
      - 网络性能监控（Network Performance Monitor, NPM）
         - 网络检测
         - 网络实时流量监控（延迟，访问量，成功率）
         - 历史数据统计、汇总、数据分析
      - 网络攻击探测
         - 通过分析异常流量来确定网络攻击行为，如DDos攻击
   - 存储
      - 云存储
      - 分布式存储
   - 服务器
      - 类型
         - CPU
         - 内存
         - 网络
         - 存储
      - 工具
         - Zabbix
         - Open-Falcon
         - Prometheus
- 日志监控
   - Fluented(收集) -> Kafaka（数据整流合并） -> Logstash（日志整理） -> Elasticsearch（日志存储和检索） -> Kibana（日志查询）

![Alt text](/docs/images/monitoring/image.png)

linux使用下图（[https://www.brendangregg.com/Perf/linux_perf_tools_full.png](https://www.brendangregg.com/Perf/linux_perf_tools_full.png)）中的各种性能检测工具来查看系统中的各个方面的性能指标。
![Alt text](/docs/images/monitoring/image-1.png)
## 监控总体架构

1. 数据采集！！

主动、被动、两种都有

2. 数据传输
- socket
- http传输
3. 数据存储

存在Mysql中，还有的存在MongoDB，OpenTSDB，InfluxDB等时序数据库中
![Alt text](/docs/images/monitoring/image-2.png)
### 指标采集子系统
主要负责，信息采集、过滤、汇总和存储
#### 指标采集
##### 通过客户端采集
通过标准协议：SNMP，JMX，IPMI<br />prom通过客户端exporter采集。exporter采集，并对数据进行检查处理，缓存，超时重试等。<br />劣势

- 需要一定的开发工作
- 侵入式安装客户端存在安全和性能风险
##### 通过标准协议：SNMP，JMX，IPMI
java应用——JMX
#### 数据传输和过滤
采集数据传输到数据存储节点——HTTP,socket连接进行点对点传输，也可以通过RabbitMQ和Kafka中间件进行传输
#### 数据存储
时序数据库（TSDB）的特点

- 一次写入多次读取
- 数据流持续平稳，数据量和采集点的数据呈正比
- 查询方式以时间为维度，数据查询通常指定一段时间，并且热数据通常是最新采集的数据
- 存储容量大，数据只读及数据压缩比很高
- 高吞吐量，高并发
### 数据处理子系统
主要负责数据分析、展现、预警、告警动作触发和告警等。
#### 数据查询
#### 数据分析
##### 性能分析
某应用特定时间段内的资源消耗情况
##### 关联分析
找出数据之间的关联

- 网络监控中，源IP和目的IP地址关联，构建网络连接拓扑
- APM监控中，相同TranceId的关联，构建出程序的调用链信息
##### 趋势分析
通过各种算法预测未来的变化趋势
#### 基于规则告警

- 阈值触发告警
- 异常告警
- 告警抑制——相同告警
- 告警收敛——根问题触发的一系列子问题
- 告警恢复
### 监控系统的发展趋势
问题：

- 监控系统提供的对单个指标的监控并不能很好地反映运行系统和程序的当前状态。
- 告警都是发生在故障之后，不能提前预警
- 告警之后，运维人员需要综合分析来确定问题，延长问题解决得时间

传统运维：性能监控、健康状态监控、应用监控等基础能力的监控<br />智能监控：具备机器学习和自动化运维能力

- 通过多种维度的指标分析定位问题的故障点：硬件、软件等多维度指标，准确迅速定位故障点
- 故障和风险预测：通过对以往故障的学习，预测未来的故障时间
   - AWS通过对大量服务器发生故障的时间、故障前的某些指标的异常等学习，进行更换服务器
   - 突发流量（历史请求的QPS，预测流量高峰，提前扩容）
- 智能处理：机器的自动化运维，不需人为干涉，机器处理故障


![运维监控系统](/docs/images/monitoring/image-3.png)
