# InfluxDB

InfluxDB 是一个开源的时序数据库。

## 版本说明

| 目录 | 版本 | 说明 |
|------|------|------|
| `influxdb2/` | 2.x + Telegraf | 最新版本，与1.x不兼容 |
| `influxdb1/` | 1.8 + Chronograf + Kapacitor | 经典版本 |

## 重要说明

**InfluxDB 1.x 和 2.x 完全不兼容！**

- 1.x: 使用 InfluxQL 查询语言
- 2.x: 使用 Flux 查询语言

## 端口

### InfluxDB 2.x

| 服务 | 端口 |
|------|------|
| InfluxDB | 8086 |
| Telegraf | - |

### InfluxDB 1.x

| 服务 | 端口 |
|------|------|
| InfluxDB | 8086 |
| Chronograf | 8888 |
| Kapacitor | 9092 |

## 连接示例

### InfluxDB 2.x

```bash
# 命令行
influx -host localhost -port 8086 -token my-super-secret-auth-token -org myorg

# Docker
docker exec -it influxdb2 influx

# HTTP API
curl http://localhost:8086/api/v2/query -X POST \
  -H "Authorization: Token my-super-secret-auth-token" \
  -d 'from(bucket:"mybucket") |> range(start: -1h)'
```

### InfluxDB 1.x

```bash
# 命令行
influx -host localhost -port 8086 -username admin -password admin123456

# Docker
docker exec -it influxdb1 influx

# HTTP API
curl -G 'http://localhost:8086/query?db=telegraf' \
  --data-urlencode "q=SELECT * FROM cpu_usage LIMIT 5"
```

## 环境变量

### InfluxDB 2.x

| 变量 | 默认值 |
|------|--------|
| INFLUXDB_USER | admin |
| INFLUXDB_PASSWORD | admin123456 |
| INFLUXDB_ORG | myorg |
| INFLUXDB_BUCKET | mybucket |
| INFLUXDB_TOKEN | my-super-secret-auth-token |

### InfluxDB 1.x

| 变量 | 默认值 |
|------|--------|
| INFLUXDB_DB | telegraf |
| INFLUXDB_ADMIN_USER | admin |
| INFLUXDB_ADMIN_PASSWORD | admin123456 |
| INFLUXDB_USER | telegraf |
| INFLUXDB_USER_PASSWORD | telegraf123456 |

## 管理界面

- InfluxDB 2.x: http://localhost:8086
- Chronograf (1.x): http://localhost:8888

## Telegraf 配置

InfluxDB 2.x 配套 Telegraf 配置文件在 `./telegraf.conf`。

## 持久化

- InfluxDB数据: `influxdb_data`
- Kapacitor数据: `kapacitor_data`
