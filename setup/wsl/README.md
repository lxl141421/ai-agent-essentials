# WSL2 配置指南

> Windows Subsystem for Linux 2 上部署 AI Agent 的踩坑记录与最佳实践。

---

## 目录

- [网络问题](#网络问题)
- [API Key 管理](#api-key-管理)
- [代理软件冲突](#代理软件冲突)
- [本地模型连接](#本地模型连接)
- [文件系统性能](#文件系统性能)
- [GPU 直通](#gpu-直通)
- [常见报错](#常见报错)

---

## 网络问题

### localhost 不是指你以为的那个

**问题：** WSL2 是一个轻量级虚拟机，有自己的网络栈。`localhost` 在 WSL 里指向的是 WSL 自己，不是 Windows 主机。

**症状：** 在 WSL 里 `curl http://localhost:1234` 连不上 Windows 上跑的 LM Studio / Ollama。

**解决：**

```bash
# 方法1：使用 host.docker.internal（Docker Desktop 用户）
curl http://host.docker.internal:1234/v1/models

# 方法2：获取 Windows 主机 IP
cat /etc/resolv.conf | grep nameserver
# 输出类似：nameserver 172.25.112.1
curl http://172.25.112.1:1234/v1/models

# 方法3：在 .bashrc 里设个变量
echo 'export WIN_HOST=$(cat /etc/resolv.conf | grep nameserver | awk "{print \$2}")' >> ~/.bashrc
source ~/.bashrc
curl http://$WIN_HOST:1234/v1/models
```

**Hermes Agent 配置：**
```bash
# 如果 localhost 不行，用 Windows IP
hermes config set model.base_url http://172.25.112.1:1234/v1
```

### WSL2 端口转发

**问题：** Windows 上的服务需要能被 WSL 访问到。

**解决：** 一般情况下 WSL2 可以直接访问 Windows 的 IP。如果不行：

```powershell
# Windows PowerShell（管理员）
# 检查防火墙是否阻止了
netsh advfirewall firewall add rule name="WSL" dir=in action=allow protocol=any localip=172.25.0.0/16
```

---

## API Key 管理

### 放哪里？

**推荐：** `~/.env` 文件（Hermes Agent 默认读取）

```bash
# ~/.env
OPENROUTER_API_KEY=sk-or-v1-xxxxx
DASHSCOPE_API_KEY=sk-xxxxx
ANTHROPIC_API_KEY=sk-ant-xxxxx
```

**不推荐：**
- ❌ 硬编码到 `config.yaml`（会被 Git 追踪）
- ❌ 写在 `.bashrc` 里（太乱）
- ❌ 放在 Windows 侧的文件（路径转换麻烦）

### WSL 和 Windows 共享 Key

如果你想让 WSL 和 Windows 侧的程序用同一套 Key：

```bash
# 方法1：符号链接（推荐）
ln -s /mnt/c/Users/你的用户名/.env ~/.env

# 方法2：在 WSL 的 .env 里引用 Windows 路径
# 不推荐，路径转换容易出问题
```

### 环境变量加载顺序

```
WSL 启动 shell
  → ~/.bashrc
    → source ~/.env（如果在里面写了）
      → 或者 Hermes 自动读取 ~/.hermes/.env
```

**Hermes Agent 用户：** 直接编辑 `~/.hermes/.env`，Hermes 会自动加载。

---

## 代理软件冲突

### Clash / V2Ray / 其他代理

**问题：** Windows 上的代理软件会设置系统代理，WSL2 可能会受到影响。

**症状：**
- API 请求超时
- 连接被重置
- SSL 证书错误
- 速度异常慢

**诊断：**

```bash
# 检查是否走了代理
curl -v https://api.openai.com/v1/models 2>&1 | grep -i "proxy\|connect"

# 检查环境变量
echo $http_proxy
echo $https_proxy
echo $HTTP_PROXY
echo $HTTPS_PROXY
```

**解决方案：**

```bash
# 方案1：清除代理环境变量
unset http_proxy https_proxy HTTP_PROXY HTTPS_PROXY

# 方案2：在 .bashrc 里明确不走代理
echo 'unset http_proxy https_proxy HTTP_PROXY HTTPS_PROXY' >> ~/.bashrc

# 方案3：如果需要代理才能访问外网，设置正确的代理地址
# 假设 Clash 在 Windows 上跑，端口 7890
export http_proxy=http://$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):7890
export https_proxy=$http_proxy
```

### 代理对本地模型的影响

**问题：** 连接本地 LM Studio 时，代理会把 `localhost` 的请求也转发出去。

**解决：**

```bash
# 不代理本地地址
export no_proxy=localhost,127.0.0.1,host.docker.internal
export NO_PROXY=$no_proxy

# 或者在 .bashrc 里永久设置
echo 'export no_proxy=localhost,127.0.0.1,host.docker.internal' >> ~/.bashrc
```

### 常见代理软件的本地端口

| 代理软件 | 默认端口 |
|---------|---------|
| Clash | 7890 |
| V2Ray | 10809 |
| Shadowsocks | 1080 |
| Surge | 6152 |

---

## 本地模型连接

### LM Studio（Windows 侧运行）

```bash
# 1. 确认 LM Studio 服务器已启动（Developer → Start Server）
# 2. 在 WSL 里测试连接

# 获取 Windows IP
WIN_IP=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}')

# 测试 API
curl http://$WIN_IP:1234/v1/models

# 配置 Hermes
hermes config set model.base_url http://$WIN_IP:1234/v1
hermes config set model.api_key lm-studio
```

### Ollama（WSL 内运行）

```bash
# 如果 Ollama 装在 WSL 里，直接用 localhost
curl http://localhost:11434/api/tags

# 配置 Hermes
hermes config set model.base_url http://localhost:11434/v1
```

### Ollama（Windows 侧运行）

```bash
# 同 LM Studio，用 Windows IP
WIN_IP=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}')
curl http://$WIN_IP:11434/api/tags
```

---

## 文件系统性能

### 跨文件系统很慢

**问题：** WSL 访问 `/mnt/c/`（Windows 文件系统）比访问原生 Linux 文件系统慢 5-10 倍。

**建议：**
- ✅ 项目文件放在 WSL 原生文件系统（`~/projects/`）
- ✅ 只在需要时访问 `/mnt/c/`
- ❌ 不要在 `/mnt/c/` 上跑 `npm install`、`pip install` 等
- ❌ 不要在 `/mnt/c/` 上跑 Git 操作

```bash
# 好：在 WSL 原生文件系统工作
cd ~/projects/my-agent && npm install

# 差：在 Windows 文件系统工作
cd /mnt/c/Users/me/projects/my-agent && npm install  # 慢！
```

### Windows 访问 WSL 文件

```
# Windows 文件管理器地址栏输入：
\\wsl$\Ubuntu\home\你的用户名\
```

---

## GPU 直通

### NVIDIA GPU

WSL2 支持 NVIDIA GPU 直通（CUDA）：

```bash
# 检查 GPU 是否可用
nvidia-smi

# 如果没有，需要：
# 1. Windows 上安装 NVIDIA 驱动（>= 470.76）
# 2. WSL 里安装 CUDA toolkit
# 3. 不要装 WSL 内的 GPU 驱动！用 Windows 的驱动
```

### AMD GPU

WSL2 对 AMD GPU 的支持有限：

```bash
# 检查是否有 ROCm 支持
rocm-smi 2>/dev/null || echo "ROCm 不可用"

# AMD 用户建议：
# - 在 Windows 侧运行 LM Studio（GPU 加速）
# - WSL 侧通过 API 连接
```

### 集成显卡（如 Radeon 8060S）

- Windows 侧的 LM Studio 可以利用集成显卡
- WSL 内一般无法直接使用集成显卡加速
- 建议：Windows 跑模型 + WSL 跑 Agent 逻辑

---

## 常见报错

### `ECONNREFUSED`

```
Error: connect ECONNREFUSED 127.0.0.1:1234
```

**原因：** WSL 的 localhost 连不到 Windows 的服务。
**解决：** 用 Windows IP 替代 localhost（见上方网络问题）。

### `ETIMEDOUT`

```
Error: connect ETIMEDOUT
```

**原因：** 代理干扰，或防火墙阻止。
**解决：** 检查代理设置，清除代理环境变量。

### `SSL: CERTIFICATE_VERIFY_FAILED`

```
SSL: CERTIFICATE_VERIFY_FAILED
```

**原因：** 代理软件的 SSL 中间人劫持。
**解决：**

```bash
# 临时方案（不安全，仅测试用）
export CURL_SSL_NO_VERIFY=1
export PYTHONHTTPSVERIFY=0

# 正确方案：将代理的 CA 证书加入信任列表
# 或者配置代理不要劫持 API 域名
```

### `Permission denied`

```
Permission denied: '/mnt/c/...'
```

**原因：** WSL 的权限模型和 Windows NTFS 不兼容。
**解决：**

```bash
# 在 /etc/wsl.conf 中配置
[automount]
options = "metadata,umask=22,fmask=11"
```

---

## 最佳实践清单

```
✅ 项目文件放在 ~/ 下，不要放 /mnt/c/
✅ API Key 存 ~/.hermes/.env
✅ 清除代理环境变量（或正确配置 no_proxy）
✅ 本地模型用 Windows IP 而非 localhost
✅ 定期检查 /etc/resolv.conf（WSL 重启后 IP 可能变）
✅ Windows 侧装 GPU 驱动，WSL 侧不要重复装
✅ 用 systemd 管理 gateway 服务（需要 wsl.conf 开启 systemd）
```

---

*如果你遇到了本文档未覆盖的问题，欢迎提交 PR 补充！*
