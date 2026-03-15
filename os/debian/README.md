# Debian

Debian 容器，用于开发测试。

## 版本

- `debian12/`: Debian 12 (Bookworm)

## 快速启动

```bash
cd debian12 && docker-compose up -d
```

## 使用

```bash
# 进入容器
docker exec -it debian12 bash

# 更新包
docker exec debian12 apt update

# 安装软件
docker exec debian12 apt install -y curl wget vim

# 运行命令
docker exec debian12 <command>
```

## 挂载目录

| 目录 | 说明 |
|------|------|
| ./data | 数据目录 |

## 自定义

### 添加软件

编辑 `docker-compose.yml`:

```yaml
services:
  debian:
    image: debian:12
    # ... 其他配置
    command: >
      bash -c "
      apt update &&
      apt install -y curl wget vim git &&
      tail -f /dev/null
      "
```

### 初始化脚本

创建 `./init.sh`:

```bash
#!/bin/bash
apt update
apt install -y curl wget vim git
# 其他初始化命令
```

然后在 docker-compose.yml 中挂载:

```yaml
volumes:
  - ./init.sh:/init.sh:ro
command: bash /init.sh && tail -f /dev/null
```

## 常用软件安装

```bash
# 开发工具
apt install -y build-essential git curl wget vim

# Python
apt install -y python3 python3-pip

# Node.js (需要添加源)
curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
apt install -y nodejs

# 网络工具
apt install -y net-tools iputils-ping curl wget

# 系统工具
apt install -y htop iotop
```

## 持久化

- debian_home: /root 目录
