# Windows 原生配置指南

> Windows 原生环境（非 WSL）下部署 AI Agent 的注意事项。

---

## 路径与编码

### 路径分隔符

```python
# Python 中优先用 pathlib
from pathlib import Path
config = Path.home() / ".hermes" / "config.yaml"

# 或用正斜杠（Windows 也支持）
path = "C:/Users/username/.hermes/config.yaml"
```

### UTF-8 编码

- `config.yaml` 必须用 **UTF-8 无 BOM** 编码保存
- Windows 记事本默认加 BOM，会导致 YAML 解析失败
- 推荐用 VS Code 或 `hermes config edit`

## Python 环境

```powershell
# 推荐用官方 Python，不要用 Microsoft Store 版
winget install Python.Python.3.12

# 或者用 uv
pip install uv
uv venv
```

## 常见问题

### PowerShell 执行策略

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### 防火墙

Windows 防火墙可能阻止本地服务通信，首次运行时注意允许。

---

*待补充：更多 Windows 踩坑记录。欢迎提交 PR！*
