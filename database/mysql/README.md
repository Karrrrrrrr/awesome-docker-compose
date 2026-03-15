# MySQL

MySQL 是最流行的关系型数据库管理系统之一。

## 版本说明

| 目录 | 版本 | 说明 |
|------|------|------|
| `mysql8/` | 8.0 | 最新LTS版本，推荐使用 |
| `mysql5/` | 5.7 | 经典版本，兼容旧项目 |
| `mysql8-bitnami/` | 8.0 (Bitnami) | 更安全精简的生产级镜像 |

## 快速启动

```bash
# MySQL 8.0
cd mysql8 && docker-compose up -d

# MySQL 5.7
cd mysql5 && docker-compose up -d

# Bitnami版本
cd mysql8-bitnami && docker-compose up -d
```

## 端口

| 服务 | 端口 | 说明 |
|------|------|------|
| MySQL | 3306 | 数据库端口 |
| phpMyAdmin | 8080 | Web管理界面 |

## 环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| MYSQL_ROOT_PASSWORD | root123456 | root用户密码 |
| MYSQL_DATABASE | app | 默认数据库 |
| MYSQL_USER | app | 普通用户名 |
| MYSQL_PASSWORD | app123456 | 普通用户密码 |
| MYSQL_PORT | 3306 | 数据库端口 |
| PMA_PORT | 8080 | phpMyAdmin端口 |

## 默认账号

| 用户 | 密码 | 权限 |
|------|------|------|
| root | root123456 | 超级管理员 |
| app | app123456 | 普通用户 |

## 连接示例

```bash
# 命令行连接
mysql -h 127.0.0.1 -u root -proot123456

# Docker连接
docker exec -it mysql8 mysql -u root -proot123456
```

## 持久化

数据存储在 Docker volume `mysql_data` 中，位于 `/var/lib/mysql`。

## 健康检查

```bash
docker exec mysql8 mysqladmin ping -h localhost
```

## 常用操作

```bash
# 查看日志
docker-compose logs -f mysql

# 备份数据库
docker exec mysql8 mysqldump -u root -proot123456 --all-databases > backup.sql

# 恢复数据库
docker exec -i mysql8 mysql -u root -proot123456 < backup.sql

# 进入容器
docker exec -it mysql8 bash
```

## 配置文件

- MySQL 8: `./conf.d/` 目录下的 `.cnf` 文件
- 初始化脚本: `./init/` 目录下的 `.sql` 或 `.sh` 文件
