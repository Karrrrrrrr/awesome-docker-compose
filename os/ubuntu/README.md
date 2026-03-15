# Ubuntu

Ubuntu 容器，用于开发测试。

## 版本

- `ubuntu22/`: Ubuntu 22.04 (Jammy)

## 快速启动

```bash
cd ubuntu22 && docker-compose up -d
```

## 使用

```bash
# 进入容器
docker exec -it ubuntu22 bash

# 更新包
docker exec ubuntu22 apt update

# 安装软件
docker exec ubuntu22 apt install -y curl wget vim

# 运行命令
docker exec ubuntu22 <command>
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
  ubuntu:
    image: ubuntu:22.04
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
DEBIAN_FRONTEND=noninteractive apt install -y curl wget vim git
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

# Docker CLI
apt install -y ca-certificates gnupg
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
apt update
apt install -y docker-ce-cli

# 网络工具
apt install -y net-tools iputils-ping curl wget

# 系统工具
apt install -y htop iotop
```

## 与Debian对比

| 特性 | Ubuntu | Debian |
|------|--------|--------|
| 发布周期 | 6个月 | 2年 |
| PPA支持 | 支持 | 不支持 |
| 软件版本 | 较新 | 较稳定 |
| 社区支持 | 商业+社区 | 社区 |

## 持久化

- ubuntu_home: /root 目录
