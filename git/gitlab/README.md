# GitLab CE

GitLab 是一个完整的 DevOps 平台。

## 版本

- `gitlab-ce/`: GitLab Community Edition

## 快速启动

```bash
cd gitlab-ce && docker-compose up -d
```

## 端口

| 端口 | 说明 |
|------|------|
| 8929 | Web界面/HTTP |
| 2224 | SSH |

## 首次使用

1. 等待启动完成 (约3-5分钟)
2. 访问 http://localhost:8929
3. 设置root密码
4. 登录 (root / 您设置的密码)

## 环境变量

| 变量 | 默认值 |
|------|--------|
| GITLAB_EXTERNAL_URL | http://localhost:8929 |
| GITLAB_ROOT_PASSWORD | gitlab123456 |
| GITLAB_HTTP_PORT | 8929 |
| GITLAB_SSH_PORT | 2224 |

## 组件

| 服务 | 说明 |
|------|------|
| GitLab | 主服务 |
| PostgreSQL | 数据库 |
| Redis | 缓存 |

## 功能

- Git仓库管理
- Issue追踪
- Merge Request
- CI/CD (GitLab CI)
- Container Registry
- Wiki
- 代码审查
- 安全扫描

## GitLab CI/CD

```yaml
# .gitlab-ci.yml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  script:
    - echo "Building..."

test:
  stage: test
  script:
    - echo "Testing..."

deploy:
  stage: deploy
  script:
    - echo "Deploying..."
```

## Runner安装

```bash
# 注册Runner
docker exec gitlab-runner gitlab-runner register \
  --non-interactive \
  --url http://gitlab:8929 \
  --registration-token <TOKEN> \
  --executor docker \
  --docker-image alpine:latest
```

## 常用命令

```bash
# 查看状态
docker exec gitlab-ce gitlab-ctl status

# 重启服务
docker exec gitlab-ce gitlab-ctl restart

# 查看日志
docker exec gitlab-ce gitlab-ctl tail

# 备份
docker exec gitlab-ce gitlab-backup create

# 恢复
docker exec gitlab-ce gitlab-backup restore BACKUP=<timestamp>
```

## 配置

配置文件位于 `./config/gitlab.rb`

```ruby
# 外部URL
external_url 'http://gitlab.example.com'

# SSH端口
gitlab_rails['gitlab_shell_ssh_port'] = 2224

# 禁用不需要的服务
prometheus_monitoring['enable'] = false
grafana['enable'] = false
```

修改配置后:
```bash
docker exec gitlab-ce gitlab-ctl reconfigure
```

## 系统要求

- CPU: 4核心+
- RAM: 8GB+
- 存储: 50GB+

## 持久化

- gitlab_config: GitLab配置
- gitlab_logs: 日志
- gitlab_data: 数据
