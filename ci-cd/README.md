# CI/CD 服务

持续集成和持续部署工具。

## 服务列表

| 目录 | 服务 | 端口 | 说明 |
|------|------|------|------|
| jenkins/ | Jenkins | 8080 | 经典CI/CD服务器 |
| drone/ | Drone CI | 80, 3000 | 轻量级CI/CD |

## Jenkins

企业级 CI/CD 服务器。

### 端口

| 端口 | 说明 |
|------|------|
| 8080 | Web界面 |
| 50000 | Agent端口 |

### 首次启动

1. 访问 http://localhost:8080
2. 获取初始密码: `docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword`
3. 安装推荐插件
4. 创建管理员账户

### 常用操作

```bash
# 获取初始密码
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword

# 重启Jenkins
docker restart jenkins

# 查看日志
docker logs -f jenkins

# 安装插件
docker exec jenkins jenkins-plugin-cli --plugins git docker-workflow pipeline-stage-view
```

## Drone CI

轻量级 CI/CD，适合容器化项目。

### 组件

- Drone Server: Web界面和API
- Drone Runner: 执行流水线
- PostgreSQL: 数据存储

### 环境变量

| 变量 | 说明 |
|------|------|
| DRONE_HOST | 服务器地址 |
| DRONE_RPC_SECRET | RPC密钥 |
| DRONE_GITEA_CLIENT_ID | Gitea OAuth ID |
| DRONE_GITEA_CLIENT_SECRET | Gitea OAuth Secret |

### 配置 Git 提供商

Drone 需要配置 Git 提供商的 OAuth:

1. Gitea: 创建 OAuth应用，回调URL为 `http://drone/login`
2. GitHub: 创建OAuth App
3. GitLab: 创建Application

### 流水线示例

```yaml
# .drone.yml
kind: pipeline
type: docker
name: default

steps:
  - name: build
    image: golang:1.21
    commands:
      - go build
      - go test

  - name: docker
    image: plugins/docker
    settings:
      repo: user/app
      tags: latest
```

## 持久化

- jenkins_home: Jenkins数据
- drone_data: Drone数据
- drone_db_data: Drone数据库
