# AI 服务

本地运行的 AI/LLM 服务。

## 服务列表

| 目录 | 服务 | 端口 | 说明 |
|------|------|------|------|
| ollama/ | Ollama + Open WebUI | 11434, 3000 | 本地LLM运行环境 |
| stable-diffusion/ | Stable Diffusion | 7860 | AI图像生成 |

## Ollama

本地运行大语言模型。

### 端口

| 端口 | 服务 |
|------|------|
| 11434 | Ollama API |
| 3000 | Open WebUI |

### 下载模型

```bash
# 下载模型
docker exec ollama ollama pull llama2

# 常用模型
docker exec ollama ollama pull llama2:7b
docker exec ollama ollama pull llama2:13b
docker exec ollama ollama pull mistral
docker exec ollama ollama pull codellama
docker exec ollama ollama pull qwen:7b

# 查看已安装模型
docker exec ollama ollama list

# 删除模型
docker exec ollama ollama rm llama2
```

### API 调用

```bash
# 生成
curl http://localhost:11434/api/generate -d '{
  "model": "llama2",
  "prompt": "Hello, how are you?"
}'

# 聊天
curl http://localhost:11434/api/chat -d '{
  "model": "llama2",
  "messages": [
    {"role": "user", "content": "Hello"}
  ]
}'
```

### Open WebUI

访问 http://localhost:3000

默认账号首次访问时创建。

## Stable Diffusion

AI 图像生成。

### 端口

| 端口 | 说明 |
|------|------|
| 7860 | Web界面 |

### GPU 支持

需要 NVIDIA GPU:

```yaml
deploy:
  resources:
    reservations:
      devices:
        - driver: nvidia
          count: 1
          capabilities: [gpu]
```

### 模型

模型文件放置在 `./models/` 目录:

- Checkpoints: `./models/Stable-diffusion/`
- LoRA: `./models/Lora/`
- VAE: `./models/VAE/`

### 常用参数

| 参数 | 说明 |
|------|------|
| --xformers | 启用xformers加速 |
| --listen | 允许外部访问 |
| --enable-insecure-extension-access | 允许安装扩展 |

## 硬件要求

### Ollama

- CPU: 8核心以上
- RAM: 16GB+ (7B模型), 32GB+ (13B模型)
- GPU: 可选，NVIDIA 8GB+显存

### Stable Diffusion

- GPU: NVIDIA 8GB+显存
- RAM: 16GB+
- 存储: 50GB+ (模型文件)

## 持久化

- ollama_data: Ollama模型
- open_webui_data: WebUI数据
- sd_models: SD模型
- sd_outputs: 生成图片
