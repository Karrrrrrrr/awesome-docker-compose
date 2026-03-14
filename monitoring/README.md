# 监控服务

可观测性工具集，包括监控、日志、追踪。

## 服务列表

| 目录 | 服务 | 端口 | 说明 |
|------|------|------|------|
| prometheus-grafana/ | Prometheus + Grafana | 9090, 3000 | 完整监控方案 |
| dozzle/ | Dozzle | 9999 | 轻量级Docker日志查看 |
| loki/ | Loki + Promtail | 3100 | 日志聚合系统 |
| promtail/ | Promtail | 9080 | 日志收集代理 |

## Prometheus + Grafana

完整的监控解决方案。

### 组件

| 组件 | 端口 | 说明 |
|------|------|------|
| Prometheus | 9090 | 指标收集 |
| Grafana | 3000 | 可视化面板 |
| Node Exporter | 9100 | 主机监控 |
| cAdvisor | 8080 | 容器监控 |
| Alertmanager | 9093 | 告警管理 |

### 快速启动

```bash
cd prometheus-grafana && docker-compose up -d
```

### 访问

- Grafana: http://localhost:3000 (admin/admin123456)
- Prometheus: http://localhost:9090
- Alertmanager: http://localhost:9093

### 配置

- Prometheus: `./prometheus.yml`
- 告警规则: `./alerts/`
- Alertmanager: `./alertmanager.yml`

## Dozzle

实时查看 Docker 容器日志。

### 特点

- 轻量级 (单容器)
- 实时日志流
- 支持多容器
- 基础认证

### 访问

http://localhost:9999 (admin/admin123456)

## Loki + Promtail

Grafana Loki 是一个日志聚合系统。

### 架构

```
应用 -> Promtail -> Loki -> Grafana
```

### 快速启动

```bash
cd loki && docker-compose up -d
```

### 配置

- Loki: `./loki-config.yaml`
- Promtail: `./promtail-config.yml`

### 查询日志 (LogQL)

```
# 查看所有日志
{job="varlogs"}

# 过滤
{job="varlogs"} |= "error"

# 正则
{job="varlogs"} |~ "error.*timeout"
```

## 常用操作

```bash
# 查看Prometheus目标
curl http://localhost:9090/api/v1/targets

# 查看Loki标签
curl http://localhost:3100/loki/api/v1/labels

# 查看Alertmanager告警
curl http://localhost:9093/api/v1/alerts
```

## 持久化

- prometheus_data: Prometheus数据
- grafana_data: Grafana配置
- loki_data: Loki日志
- alertmanager_data: 告警数据
