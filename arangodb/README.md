# ArangoDB

ArangoDB 是一个多模型数据库，支持文档、图和键值对。

## 版本

| 目录 | 版本 | 说明 |
|------|------|------|
| arangodb3/ | 3.12 | 最新版本 |
| arangodb2/ | 2.8 | 旧版本 |

## 快速启动

```bash
cd arangodb3 && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 8529 | Web界面/API |
| 8530 | (v2.x端口) |

## 环境变量

| 变量 | 默认值 |
|------|--------|
| ARANGO_ROOT_PASSWORD | root123456 |
| ARANGO_STORAGE_ENGINE | rocksdb |

## Web界面

访问 http://localhost:8529

登录: root / root123456

## 数据模型

### 文档存储

```javascript
// 创建集合
db._createDocumentCollection('users')

// 插入文档
db.users.save({name: 'Alice', age: 30})

// 查询
db.users.byExample({name: 'Alice'})
```

### 图数据库

```javascript
// 创建边集合
db._createEdgeCollection('knows')

// 插入边
db.knows.save('users/1', 'users/2', {since: 2020})

// 图遍历
FOR v, e, p IN 1..2 OUTBOUND 'users/1' knows
  RETURN {user: v.name, path: p}
```

### 键值存储

```javascript
// 设置
db._collection('kv').save({_key: 'cache1', value: 'data'})

// 获取
db._collection('kv').document('cache1')
```

## AQL查询语言

```aql
// 基本查询
FOR u IN users
  FILTER u.age > 25
  RETURN u.name

// 排序和限制
FOR u IN users
  SORT u.age DESC
  LIMIT 10
  RETURN u

// 聚合
FOR u IN users
  COLLECT ageGroup = FLOOR(u.age / 10) * 10
  AGGREGATE count = COUNT()
  RETURN {ageGroup, count}

// JOIN
FOR u IN users
  FOR o IN orders
    FILTER u._id == o.userId
    RETURN {user: u.name, order: o.items}

// 图遍历
FOR v, e, p IN 1..3 OUTBOUND 'users/1' GRAPH 'social'
  RETURN p
```

## REST API

```bash
# 创建数据库
curl -X POST --user root:root123456 \
  -d '{"name":"mydb"}' \
  http://localhost:8529/_api/database

# 创建集合
curl -X POST --user root:root123456 \
  -d '{"name":"users","type":2}' \
  http://localhost:8529/_api/collection

# 插入文档
curl -X POST --user root:root123456 \
  -d '{"name":"Alice","age":30}' \
  http://localhost:8529/_api/document/users

# AQL查询
curl -X POST --user root:root123456 \
  -d '{"query":"FOR u IN users RETURN u"}' \
  http://localhost:8529/_api/cursor
```

## 备份

```bash
# 备份
docker exec arangodb3 arangodump --output-directory /tmp/backup

# 恢复
docker exec arangodb3 arangorestore --input-directory /tmp/backup
```

## 持久化

- arangodb_data: 数据
- arangodb_apps: 应用
