# 数据库工具

数据库管理和Web界面工具。

## 服务列表

| 目录 | 服务 | 端口 | 说明 |
|------|------|------|------|
| adminer/ | Adminer | 8080 | 轻量级数据库管理 |
| cloudbeaver/ | CloudBeaver | 8978 | 企业级数据库管理 |
| pgweb/ | pgweb | 8081 | PostgreSQL Web管理 |

## Adminer

支持多种数据库的轻量级管理工具。

### 支持数据库

- MySQL
- PostgreSQL
- SQLite
- MS SQL
- Oracle
- SimpleDB
- MongoDB

### 使用

1. 访问 http://localhost:8080
2. 选择数据库类型
3. 输入连接信息
4. 登录管理

### 连接示例

```
MySQL:
  Server: mysql
  Username: root
  Password: root123456
  Database: app

PostgreSQL:
  Server: postgres
  Username: postgres
  Password: postgres123456
  Database: app
```

## CloudBeaver

DBeaver 的 Web 版本，### 特点

- 支持多种数据库
- SQL编辑器
- 数据导入导出
- ER图生成
- 团队协作

### 使用

1. 访问 http://localhost:8978
2. 首次使用创建管理员账户
3. 添加数据库连接

### 环境变量

| 变量 | 默认值 |
|------|--------|
| CB_ADMIN_NAME | admin |
| CB_ADMIN_PASSWORD | admin123456 |

## pgweb

PostgreSQL 专用 Web 知询界面。

### 使用

1. 访问 http://localhost:8081
2. 输入数据库连接信息

### 连接字符串

```
postgres://postgres:postgres123456@localhost:5432/app?sslmode=disable
```

## 持久化

- cloudbeaver_data: CloudBeaver配置
