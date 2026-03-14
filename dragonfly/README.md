# Dragonfly

Dragonfly 是一个高性能的 Redis 替代品，完全兼容 Redis 协议。

## 快速启动

```bash
cd dragonfly && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 6379 | Redis协议端口 |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| DFLY_requirepass | dragonfly123456 |
| DFLY_maxmemory | 4gb |
| DRAGONFLY_PORT | 6379 |

## 特点

- **高性能**: 单线程处理可达 1M QPS
- **内存效率**: 相比 Redis 节省 30% 内存
- **兼容性**: 完全兼容 Redis 协议
- **多线程**: 支持多核并行处理

## 连接方式

### Redis CLI

```bash
# 无密码
redis-cli -h localhost -p 6379

# 有密码
redis-cli -h localhost -p 6379 -a dragonfly123456
```

### Docker

```bash
docker exec -it dragonfly redis-cli -a dragonfly123456
```

### Python

```python
import redis

r = redis.Redis(
    host='localhost',
    port=6379,
    password='dragonfly123456'
)

r.set('key', 'value')
print(r.get('key'))
```

### Node.js

```javascript
const Redis = require('ioredis')

const redis = new Redis({
    host: 'localhost',
    port: 6379,
    password: 'dragonfly123456'
})

await redis.set('key', 'value')
const value = await redis.get('key')
```

## 与Redis对比

| 特性 | Dragonfly | Redis |
|------|-----------|-------|
| 单线程QPS | ~1M | ~100K |
| 内存使用 | 较低 | 较高 |
| 多线程 | 支持 | 需Cluster |
| 兼容性 | Redis协议 | 原生 |

## 支持的命令

Dragonfly 支持大部分 Redis 命令:

- 字符串: SET, GET, MSET, MGET, INCR, DECR...
- 哈希: HSET, HGET, HMSET, HMGET...
- 列表: LPUSH, RPUSH, LRANGE, LPOP, RPOP...
- 集合: SADD, SREM, SMEMBERS, SINTER...
- 有序集合: ZADD, ZREM, ZRANGE, ZSCORE...
- 位图: SETBIT, GETBIT, BITCOUNT...
- HyperLogLog: PFADD, PFCOUNT...
- 流: XADD, XREAD, XRANGE...
- Geo: GEOADD, GEODIST, GEORADIUS...

## 健康检查

```bash
docker exec dragonfly redis-cli -a dragonfly123456 ping
```

## 常用操作

```bash
# 查看信息
docker exec dragonfly redis-cli -a dragonfly123456 INFO

# 查看内存使用
docker exec dragonfly redis-cli -a dragonfly123456 MEMORY STATS

# 查看客户端连接
docker exec dragonfly redis-cli -a dragonfly123456 CLIENT LIST

# 监控命令
docker exec dragonfly redis-cli -a dragonfly123456 MONITOR
```

## 持久化

Dragonfly 支持 RDB 和 AOF:

```bash
# RDB快照
docker exec dragonfly redis-cli -a dragonfly123456 BGSAVE

# AOF重写
docker exec dragonfly redis-cli -a dragonfly123456 BGREWRITEAOF
```

## 数据卷

- dragonfly_data: 数据存储
