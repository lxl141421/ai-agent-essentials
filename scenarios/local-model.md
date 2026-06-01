# 本地模型玩家

> 数据不出本机，免费用到天荒地老。

---

## 你的需求

- 数据完全本地处理
- 不想付 API 费用
- 有 GPU 或大内存
- 愿意折腾

## 推荐配置

### 必装

```bash
# 选一个本地推理方案
# 方案A: Ollama（最简单）
curl -fsSL https://ollama.com/install.sh | sh
ollama pull qwen3.6:latest

# 方案B: LM Studio（图形界面）
# 下载: https://lmstudio.ai/

# 方案C: llama.cpp（最灵活）
brew install llama.cpp
```

- **Ollama / LM Studio / llama.cpp** - 本地模型推理
- **终端 + 文件工具** - 框架自带

### 加装

```bash
pip install mem0 browser-use
```

- **mem0** - 本地记忆（配置为本地存储模式）
- **browser-use** - 本地模型 + 浏览器自动化

### 预算

- API 费用: $0
- 硬件: 需要 16GB+ 内存，或 8GB+ 显存

## 推荐模型（2026年）

| 模型 | 大小 | 适合场景 |
|------|------|---------|
| Qwen3.6-35B-A3B (MoE) | ~5GB (Q4) | 日常对话、代码 |
| DeepSeek V3 (蒸馏版) | ~8GB (Q4) | 推理、数学 |
| Llama 4 Scout | ~10GB (Q4) | 通用、英文 |

## 注意事项

- 本地模型的工具调用能力有限，复杂任务可能需要手动引导
- Context Length 从 4096 开始，慢慢调大
- 首次加载模型需要 30-60 秒
