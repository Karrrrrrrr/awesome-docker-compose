# MongoDB

MongoDB 是一个基于分布式文件存储的数据库，属于 NoSQL 数据库。

## 版本说明

| 目录 | 版本 | 说明 |
|------|------|------|
| `mongo7/` | 7.0 | 最新版本 |
| `mongo6/` | 6.0 | 稳定版本 |
| `mongo7-bitnami/` | 7.0 (Bitnami) | 生产级镜像 |

## 快速启动

```bash
cd mongo7 && docker-compose up -d
```

## 端口

| 服务 | 端口 |
|------|------|
| MongoDB | 27017 |
| Mongo Express | 8081 |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| MONGO_ROOT_USER | root |
| MONGO_ROOT_PASSWORD | root123456 |
| MONGO_DATABASE | admin |
| MONGO_EXPRESS_PORT | 8081 |

## 连接示例

```bash
# 命令行
mongosh "mongodb://root:root123456@localhost:27017"

# Docker
docker exec -it mongo7 mongosh -u root -p root123456
```

## 连接字符串

```
mongodb://root:root123456@localhost:27017/app?authSource=admin
```

## 常用操作

```bash
# 备份
docker exec mongo7 mongodump --uri="mongodb://root:root123456@localhost:27017" --out=/backup

# 恢复
docker exec mongo7 mongorestore --uri="mongodb://root:root123456@localhost:27017" /backup

# 查看数据库
docker exec mongo7 mongosh -u root -p root123456 --eval "show dbs"
```
