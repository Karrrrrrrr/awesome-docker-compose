# Gitea

Gitea 是一个轻量级的 Git 服务。

## 版本

- `gitea1/`: Gitea 1.x + PostgreSQL
- `gitea1-bitnami/`: Gitea 1.x (Bitnami镜像)

## 快速启动

```bash
cd gitea1 && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 3000 | Web界面/HTTP |
| 2222 | SSH |

## 组件

| 服务 | 端口 | 说明 |
|------|------|------|
| gitea | 3000, 2222 | Git服务 |
| gitea-db | 5432 | PostgreSQL数据库 |

## 首次使用

1. 访问 http://localhost:3000
2. 完成初始配置 (自动跳转)
3. 创建管理员账户
4. 开始使用

## 环境变量

| 变量 | 默认值 |
|------|--------|
| GITEA_ROOT_URL | http://localhost:3000 |
| GITEA_HTTP_PORT | 3000 |
| GITEA_SSH_PORT | 2222 |
| GITEA_DB_PASSWORD | gitea123456 |
| GITEA_SECRET_KEY | gitea-secret-key |

## 功能

- Git仓库管理
- Issue追踪
- Pull Request
- Wiki
- 代码审查
- CI/CD (Gitea Actions)
- 包管理

## SSH使用

```bash
# 添加SSH密钥
# 在Web界面: 设置 -> SSH/GPG密钥 -> 添加密钥

# 克隆仓库
git clone ssh://git@localhost:2222/user/repo.git
```

## CLI工具 (tea)

```bash
# 安装
brew install tea

# 登录
tea login add --name local --url http://localhost:3000

# 创建仓库
tea repo create myrepo

# 创建Issue
tea issues create --title "Bug" --body "Description"
```

## Webhook

```bash
# 添加Webhook
# 仓库设置 -> Webhooks -> 添加Webhook

# 支持的事件:
# - push
# - create/delete
# - issues
# - pull_request
# - release
```

## Gitea Actions

Gitea 内置 CI/CD:

```yaml
# .gitea/workflows/build.yml
name: Build
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Hello World"
```

## 备份与恢复

```bash
# 备份
docker exec gitea gitea dump -c /data/gitea/conf/app.ini

# 恢复
# 将备份文件解压到 /data/gitea/
```

## 持久化

- gitea_data: Gitea数据
- gitea_db_data: 数据库数据
