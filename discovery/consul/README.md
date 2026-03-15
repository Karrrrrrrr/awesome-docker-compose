# Consul

Consul 是一个服务网格解决方案，提供服务发现、配置和分段功能。

## 版本

- `consul1/`: Consul 1.x
- `consul-bitnami/`: Consul (Bitnami镜像)

## 快速启动

```bash
cd consul1 && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 8500 | HTTP API & UI |
| 8600 | DNS接口 (UDP/TCP) |
| 8301 | LAN Serf |
| 8302 | WAN Serf |
| 8300 | Server RPC |

## Web界面

访问 http://localhost:8500

## 功能

### 服务发现

```bash
# 注册服务
curl -X PUT -d '{
  "Name": "web",
  "Port": 80
}' http://localhost:8500/v1/agent/service/register

# 发现服务
curl http://localhost:8500/v1/catalog/service/web

# DNS查询
dig @127.0.0.1 -p 8600 web.service.consul
```

### KV存储

```bash
# 写入
curl -X PUT -d 'value' http://localhost:8500/v1/kv/mykey

# 读取
curl http://localhost:8500/v1/kv/mykey?raw

# 删除
curl -X DELETE http://localhost:8500/v1/kv/mykey
```

### 健康检查

```bash
# 查看健康状态
curl http://localhost:8500/v1/health/service/web

# 检查节点
curl http://localhost:8500/v1/health/node/consul
```

### ACL

```bash
# 启用ACL后创建Token
curl -X PUT -d '{
  "Name": "Agent Token",
  "Type": "client",
  "Rules": "node \"\" { policy = \"read\" }"
}' http://localhost:8500/v1/acl/create
```

## 常用命令

```bash
# 查看成员
docker exec consul1 consul members

# 查看服务
docker exec consul1 consul catalog services

# 查看节点
docker exec consul1 consul catalog nodes

# 查看KV
docker exec consul1 consul kv get -recurse

# 备份
docker exec consul1 consul snapshot save backup.snap

# 恢复
docker exec consul1 consul snapshot restore backup.snap
```

## 环境变量

| 变量 | 默认值 |
|------|--------|
| CONSUL_BIND_INTERFACE | eth0 |
| CONSUL_HTTP_PORT | 8500 |
| CONSUL_DNS_PORT | 8600 |

## 持久化

- consul_data: Consul数据
