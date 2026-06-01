# macOS 配置指南

> macOS 上部署 AI Agent 的配置要点。

---

## Apple Silicon (M1/M2/M3/M4)

### Metal GPU 加速

```bash
# llama.cpp 支持 Metal 加速
cmake -B build -DGGML_METAL=on
cmake --build build --config Release

# Ollama 默认支持 Metal，无需额外配置
```

### MLX 框架

Apple 自家的 ML 框架，专为 Apple Silicon 优化：

```bash
pip install mlx mlx-lm
# 用 MLX 跑本地模型，比 llama.cpp 在 Mac 上更快
```

## 路径注意

- Python 路径：`/opt/homebrew/bin/python3`（Apple Silicon）或 `/usr/local/bin/python3`（Intel）
- Homebrew 路径：`/opt/homebrew/`（Apple Silicon）或 `/usr/local/`（Intel）
- 不要混用系统 Python 和 Homebrew Python

## 常见问题

### Xcode Command Line Tools

```bash
# 很多工具需要
xcode-select --install
```

### Homebrew 权限

```bash
# 如果遇到权限问题
sudo chown -R $(whoami) /opt/homebrew/*
```

---

*待补充：更多 macOS 踩坑记录。欢迎提交 PR！*
