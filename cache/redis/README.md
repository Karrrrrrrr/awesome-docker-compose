# Redis

Redis 是一个高性能的 key-value 内存数据库。

## 版本说明

| 目录 | 版本 | 说明 |
|------|------|------|
| `redis7/` | 7.x | 最新版本 |
| `redis6/` | 6.x | 稳定版本 |
| `redis7-bitnami/` | 7.x (Bitnami) | 生产级镜像 |
| `redis-cluster/` | 7.x Cluster | 集群模式 (6节点) |
| `redis-sentinel/` | 7.x Sentinel | 高可用模式 |

## 快速启动

```bash
# 单机版
cd redis7 && docker-compose up -d

# 集群模式
cd redis-cluster && docker-compose up -d

# Sentinel高可用
cd redis-sentinel && docker-compose up -d
```

## 端口

| 模式 | 端口 | 说明 |
|------|------|------|
| 单机 | 6379 | Redis服务 |
| Cluster | 7001-7006 | 6个节点 |
| Sentinel | 6379, 26379-26381 | 主从 + 3个Sentinel |
| Redis Commander | 8081 | Web管理界面 |

## 连接示例

```bash
# 命令行
redis-cli -h 127.0.0.1 -p 6379

# 带密码
redis-cli -h 127.0.0.1 -p 6379 -a your_password

# Docker
docker exec -it redis7 redis-cli
```

## 集群操作

```bash
# 查看集群状态
docker exec redis-cluster-1 redis-cli cluster info

# 查看节点
docker exec redis-cluster-1 redis-cli cluster nodes
```

## Sentinel 操作

```bash
# 查看主节点
redis-cli -p 26379 sentinel master mymaster

# 查看从节点
redis-cli -p 26379 sentinel replicas mymaster
```

## 持久化

- RDB: `dump.rdb`
- AOF: `appendonly.aof`

配置在 `redis.conf` 中。
