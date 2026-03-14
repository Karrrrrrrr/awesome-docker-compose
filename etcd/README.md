# Etcd

Etcd 是一个分布式键值存储，用于配置共享和服务发现。

## 版本

- `etcd3/`: Etcd 3.x
- `etcd2/`: Etcd 2.x (旧版本)

## 快速启动

```bash
cd etcd3 && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 2379 | 客户端通信 |
| 2380 | 节点间通信 |
| 8080 | etcdkeeper Web界面 |

## etcdkeeper

Web管理界面，访问 http://localhost:8080

## 使用

### etcdctl

```bash
# 设置键值
docker exec etcd3 etcdctl put /config/app "value"

# 获取键值
docker exec etcd3 etcdctl get /config/app

# 获取所有键
docker exec etcd3 etcdctl get "" --prefix

# 删除键
docker exec etcd3 etcdctl del /config/app

# 监听变化
docker exec etcd3 etcdctl watch /config/

# 租约 (TTL)
docker exec etcd3 etcdctl lease grant 60
docker exec etcd3 etcdctl put --lease=<lease-id> /config/temp "temp-value"
```

### HTTP API

```bash
# 设置键值
curl -X PUT http://localhost:2379/v3/kv/put \
  -d '{"key":"Zm9v","value":"YmFy"}'

# 获取键值
curl -X POST http://localhost:2379/v3/kv/range \
  -d '{"key":"Zm9v"}'

# 获取所有键
curl -X POST http://localhost:2379/v3/kv/range \
  -d '{"key":"","range_end":"AA=="}'
```

### 健康检查

```bash
# 集群健康
docker exec etcd3 etcdctl endpoint health

# 集群状态
docker exec etcd3 etcdctl endpoint status

# 成员列表
docker exec etcd3 etcdctl member list
```

## 环境变量

| 变量 | 默认值 |
|------|--------|
| ETCD_CLIENT_PORT | 2379 |
| ETCD_PEER_PORT | 2380 |
| ETCDKEEPER_PORT | 8080 |

## 持久化

- etcd_data: Etcd数据
