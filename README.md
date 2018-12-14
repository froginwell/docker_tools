# docker_tools

docker 常用基础设施. 这个库用来快速部署 log, 监控 等工具.

#### log

log 工具选用的是 rsyslog, 这是出于性能的考虑.

#### 监控

监控工具选用的是 telegraf + influxdb + grafana + statsd.
statsd 用的是 https://github.com/statsite/statsite 这个版本, 它是用 c 实现的,
性能比较高.

# 安装

## 一、初始化 docker 环境

创建 swarm 集群

```
docker swarm init
docker node update --label-add rsyslog=true <NODE-ID>
docker node update --label-add influxdb=true <NODE-ID>
docker node update --label-add grafana=true <NODE-ID>
docker node update --label-add statsd=true <NODE-ID>
```

创建 overlay 网络

```
docker network create --driver overlay --attachable default_overlay
```

## 二、安装 rsyslog

建立镜像

```
cd rsyslog
docker build -t froginwell/rsyslog:1.0.0 .
```

启动

```
docker stack deploy rsyslog -c docker-compose.yml
```

## 三、telegraf + influxdb + grafana + statsd

建立 statsd 镜像

```bash
git clone https://github.com/statsite/statsite.git
cd statsite
docker build -t froginwell/statsd:1.0.0 .
```

启动

```
cd tigs
docker stack deploy tigs -c docker-compose.yml
```

# 说明

## 暴露的端口号

rsyslog

| 端口号 | 类型 | 说明 |
| :---: | :---: | :---: |
| 3456 | tcp | 接收 log |

statsd

| 端口号 | 类型 | 说明 |
| :---: | :---: | :---: |
| 8125 | tcp | 接收数据 |
| 8126 | udp | 接收数据 |

influxdb

| 端口号 | 类型 | 说明 |
| :---: | :---: | :---: |
| 8086 | tcp | 接收命令 |
| 2003 | tcp | 接收通过 graphite 协议传输的数据 |

grafana

| 端口号 | 类型 | 说明 |
| :---: | :---: | :---: |
| 3000 | http | 提供 http 服务 |
