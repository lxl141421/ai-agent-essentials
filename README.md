<div align="center">

# 🧰 AI Agent Essentials

### 别挑了。照着装就行。

**不是又一个 Awesome List。是一份替你做决定的生存指南。**

[English](#english) | [中文](#中文)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Security Audited](https://img.shields.io/badge/Security-audited-orange.svg)](SECURITY.md)

*精选 · 安全审计 · 踩坑记录 · 场景匹配 · 拿来即用*

</div>

---

> **awesome-mcp-servers 列了 8000 个，awesome-llm-apps 列了 100+ 个。**
> **你只需要 5 个。**

## 为什么需要这个？

你刚入坑 AI Agent 开发，搜了一圈发现：

- 📋 **awesome-ai-agents** — 280 个项目，不知道选哪个
- 📋 **awesome-llm-apps** — 100+ 个应用，看了等于没看
- 📋 **awesome-mcp-servers** — 8000 个服务器，哪个没后门？
- 📋 **各框架官方文档** — 只说怎么装，不说你的环境有什么坑

**你真正需要的不是更多选项，而是有人替你选好。**

这个项目的价值：

| | 其他 Awesome List | 这个项目 |
|--|-------------------|---------|
| 目标 | 列出所有项目 | 帮你做决定 |
| 数量 | 每个领域 50-100 个 | 每个领域 **2-3 个** |
| 安全 | 不管 | **每个工具安全审计** |
| 踩坑 | 不管 | **真实环境踩坑记录** |
| 分类 | 按技术分 | **按你的需求分** |
| 结果 | 选择焦虑 | **5 分钟装完能用** |

---

## ⚡ 5 分钟速装清单

> **你只需要这些，就能跑起一个完整的 AI Agent。**

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│   #1  LLM 网关        LiteLLM（统一所有 API）            │
│   #2  记忆系统        mem0（跨会话记住你）                │
│   #3  知识库          LlamaIndex（查阅你的文档）          │
│   #4  浏览器          browser-use（上网操作网页）         │
│   #5  工具系统        你的框架自带（终端+文件）            │
│                                                         │
│   → 5 个工具，覆盖 90% 场景                              │
│   → 全部安全审计通过                                      │
│   → 全部 MIT/Apache 开源                                 │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

```bash
# 一键安装（Python 环境）
pip install litellm mem0 llama-index browser-use

# 配置 API Key
export OPENROUTER_API_KEY=sk-xxx  # 或其他厂商的 Key

# 跑起来
python -c "
from litellm import completion
r = completion(model='gpt-4o', messages=[{'role':'user','content':'Hello!'}])
print(r.choices[0].message.content)
"
```

**就这么简单。** 不需要纠结选什么，不需要看 10 篇对比文章。

---

## 🗺️ 完整分类地图

> 5 分钟清单之外，按需加装。

```
                    ┌─ 你是谁？ ─┐
                    │           │
          ┌─────────┴───┐   ┌──┴──────────┐
          │ 独立开发者   │   │ 小团队 3-5人 │
          └─────┬───────┘   └──┬──────────┘
                │              │
    ┌───────────┴──┐    ┌─────┴──────────┐
    │ 省钱优先      │    │ 效率优先        │
    │ ↓             │    │ ↓              │
    │ 本地模型      │    │ 云端 + 可观测性  │
    │ + Token 监控  │    │ + 多 Agent      │
    └──────────────┘    └────────────────┘
```

### 🔴 必备（没有就跑不起来）

| # | 领域 | 🥇 首选 | 🥈 备选 | 一句话 |
|---|------|--------|--------|--------|
| 1 | LLM 网关 | [LiteLLM](https://github.com/BerriAI/litellm) | [OpenRouter](https://openrouter.ai/) | 一套代码切换 100+ 厂商 |
| 2 | 工具系统 | 框架内置 | [MCP 工具服务器](https://github.com/modelcontextprotocol) | 终端 + 文件，框架自带 |
| 3 | 上下文管理 | 框架内置 | [LLMLingua](https://github.com/microsoft/LLMLingua) | 窗口有限，压缩来凑 |

### 🟡 核心（Agent 的核心竞争力）

| # | 领域 | 🥇 首选 | 🥈 备选 | 一句话 |
|---|------|--------|--------|--------|
| 4 | 记忆系统 | [mem0](https://github.com/mem0ai/mem0) | [OpenViking](https://github.com/volcengine/OpenViking) | 跨会话记住你是谁 |
| 5 | 知识库 | [LlamaIndex](https://github.com/run-llama/llama_index) | [ChromaDB](https://github.com/chroma-core/chroma) | 让 Agent 查你的文档 |
| 6 | 浏览器 | [browser-use](https://github.com/browser-use/browser-use) | [Nanobrowser](https://github.com/nanobrowser/nanobrowser) | 上网、操作网页、填表 |

### 🟢 效率（省钱省心）

| # | 领域 | 🥇 首选 | 🥈 备选 | 一句话 |
|---|------|--------|--------|--------|
| 7 | Token 监控 | LiteLLM 内置 | [Tokentap](https://github.com/jmuncor/tokentap) | 省下来的就是赚到的 |
| 8 | Prompt 管理 | Git 版本控制 | 框架内置模板 | Prompt 就是代码 |
| 9 | 多模型路由 | LiteLLM Fallback | OpenRouter 自动路由 | 挂了自动切 |

### 🔵 进阶（锦上添花）

| # | 领域 | 🥇 首选 | 🥈 备选 | 一句话 |
|---|------|--------|--------|--------|
| 10 | 多 Agent | [LangGraph](https://github.com/langchain-ai/langgraph) | [MetaGPT](https://github.com/FoundationAgents/MetaGPT) | 一个搞不定就来一群 |
| 11 | MCP 生态 | [awesome-mcp-servers](https://github.com/punkpeye/awesome-mcp-servers) | [官方 SDK](https://github.com/modelcontextprotocol) | 工具连接标准（安全分级！） |
| 12 | 可观测性 | [LangSmith](https://smith.langchain.com/) | [Phoenix](https://github.com/Arize-ai/phoenix) | 看不见就管不了 |

> 📖 **每个领域有独立的详细评测文档** → [categories/](categories/)

---

## 🛡️ 安全审计（本项目独有）

> **别人列项目，我们审项目。**

每个推荐工具都标注：

| 标记 | 含义 |
|------|------|
| ✅ 审计通过 | 代码审查无已知风险 |
| ⚠️ 需注意 | 有安全注意事项，详见文档 |
| 🔒 数据本地 | 数据不离开本机 |
| ☁️ 数据上云 | 数据经过第三方 |

### MCP 服务器安全分级

> MCP 服务器直接暴露给 LLM，**一个恶意服务器能操控你的 Agent**。

```
🟢 官方认证    Anthropic / 大厂官方出品 → 放心用
🟡 社区验证    100+ Star，活跃维护 → 可以用
🟠 社区早期    新项目，自己先审查 → 沙箱测试
🔴 未验证      无人使用 / 可疑 → 不要碰
```

详见 → [security/](security/)

---

## 🪤 踩坑记录

> **我们踩过的坑，就是你省下的时间。**

### WSL 用户（最常踩坑的群体）

```bash
# ❌ 坑1：localhost 连不上 Windows 的 LM Studio
curl http://localhost:1234/v1/models  # 超时！

# ✅ 解决：用 Windows IP
WIN_IP=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}')
curl http://$WIN_IP:1234/v1/models

# ❌ 坑2：代理软件劫持 API 请求
# Clash/V2Ray 会拦截流量，导致 SSL 错误

# ✅ 解决：清除代理环境变量
unset http_proxy https_proxy HTTP_PROXY HTTPS_PROXY
export no_proxy=localhost,127.0.0.1

# ❌ 坑3：API Key 放 config.yaml 被 Git 泄露
# ✅ 解决：放 ~/.hermes/.env，加入 .gitignore
```

### macOS 用户

```bash
# ❌ 坑：Apple Silicon 用 pip 装 llama-cpp-python 编译失败
# ✅ 解决：CMAKE_ARGS="-DGGML_METAL=on" pip install llama-cpp-python
```

### 通用坑

```bash
# ❌ 坑：context_length 设太大，内存爆了
# ✅ 解决：本地模型从 4096 开始，慢慢调大

# ❌ 坑：工具输出太长，token 疯涨
# ✅ 解决：截断工具输出，只保留关键信息
```

> 📖 **完整的踩坑大全** → [setup/](setup/)

---

## 🎯 场景匹配

> 你是谁？从哪开始？

<table>
<tr>
<td width="50%">

**🧑‍💻 独立开发者**
```
必装: LiteLLM + mem0 + 终端/文件
加装: browser-use + LlamaIndex
预算: $20-50/月
教程: scenarios/indie-developer.md
```

**🎨 前端工程师**
```
必装: browser-use + 终端/文件
加装: MCP 生态 + Prompt 管理
预算: $10-30/月
教程: scenarios/frontend-dev.md
```

**📊 数据科学家**
```
必装: LlamaIndex + 终端/文件
加装: mem0 + Token 监控
预算: $30-80/月
教程: scenarios/data-scientist.md
```

</td>
<td width="50%">

**👥 小团队（3-5人）**
```
必装: LiteLLM + mem0 + LlamaIndex
加装: LangGraph + LangSmith
预算: $100-300/月
教程: scenarios/small-team.md
```

**🏠 本地模型玩家**
```
必装: Ollama/llama.cpp + 终端/文件
加装: mem0 + browser-use
预算: 电费
教程: scenarios/local-model.md
```

**🪟 WSL 用户**
```
必装: 全部 + 网络配置
加装: 代理穿透
注意: 看踩坑记录！
教程: setup/wsl/README.md
```

</td>
</tr>
</table>

---

## 🌳 决策树

> 不知道选什么？跟着树走。

```
你想让 AI Agent 做什么？
│
├── 🖥️ 操作电脑（跑命令、改文件）
│   └── → 工具系统 = 必装（框架自带）
│
├── 🌐 上网查资料、操作网页
│   └── → browser-use
│       └── 需要登录态？→ Nanobrowser
│
├── 📚 查阅自己的文档/代码库
│   └── → LlamaIndex
│       └── < 1000 页？→ ChromaDB 够了
│
├── 🧠 记住之前的对话
│   └── → mem0
│       └── 隐私敏感？→ 本地方案
│
├── 🔀 用多个 LLM 厂商
│   └── → LiteLLM
│       └── 不想部署？→ OpenRouter
│
└── 💰 省 Token 费用
    └── → 上下文压缩 + LLMLingua
        └── 精确控制？→ Tokentap
```

---

## 📁 项目结构

```
agent-infra-guide/
│
├── README.md                      ← 你在看的这个
├── CONTRIBUTING.md                ← 贡献指南（有门槛）
├── SECURITY.md                    ← 安全审计标准
├── LICENSE                        ← MIT
│
├── categories/                    ← 12 个领域的详细评测
│   ├── 01-gateway.md              ← LLM 网关
│   ├── 02-context.md              ← 上下文管理
│   ├── 03-memory.md               ← 记忆系统
│   ├── 04-knowledge-base.md       ← 知识库 / RAG
│   ├── 05-browser.md              ← 浏览器自动化
│   ├── 06-terminal-file.md        ← 终端 & 文件工具
│   ├── 07-token-optimization.md   ← Token 监控与优化
│   ├── 08-prompt-management.md    ← Prompt 管理
│   ├── 09-multi-agent.md          ← 多 Agent 协作
│   ├── 10-mcp.md                  ← MCP 生态
│   └── 11-observability.md        ← 评估 / 可观测性
│
├── security/                      ← 安全审计报告
│   ├── mcp-servers.md             ← MCP 服务器安全分级
│   └── tool-audit.md              ← 工具安全审计
│
├── setup/                         ← 环境配置 & 踩坑记录
│   ├── wsl/README.md              ← WSL 专属踩坑大全
│   ├── macos/README.md            ← macOS（Apple Silicon）
│   ├── windows/README.md          ← Windows 原生
│   └── linux/README.md            ← Linux（GPU 驱动）
│
└── scenarios/                     ← 场景匹配方案
    ├── indie-developer.md         ← 独立开发者
    ├── frontend-dev.md            ← 前端工程师
    ├── data-scientist.md          ← 数据科学家
    ├── small-team.md              ← 小团队
    └── local-model.md             ← 本地模型玩家
```

---

## 🤝 贡献

欢迎 PR，但有门槛：

1. **必须附安全评估** —— 不接受纯功能推荐
2. **踩坑记录优先** —— 你踩过的坑 = 别人省下的时间
3. **宁缺毋滥** —— 新增必须替换一个，每领域最多 3 个
4. **真实使用** —— 推荐的工具你至少用过一周

详见 [CONTRIBUTING.md](CONTRIBUTING.md)

---

## ⭐ 觉得有用？

**给个 Star，让更多人不用再做选择题。**

---

## English

### What is this?

**Not another Awesome List.** A curated, opinionated, security-audited guide to AI Agent infrastructure.

**The problem:** awesome-ai-agents lists 280 projects. awesome-mcp-servers lists 8000. You don't need more options — you need someone to decide for you.

**The solution:** each category, 2-3 picks. Every tool security-audited. Real pitfall documentation. Scenario-based recommendations.

| | Other Awesome Lists | This Project |
|--|-------------------|---------|
| Goal | List everything | Help you decide |
| Count | 50-100 per category | **2-3 per category** |
| Security | Not covered | **Every tool audited** |
| Pitfalls | Not covered | **Real-world solutions** |
| Result | Analysis paralysis | **5-minute setup** |

### The 5-Minute Stack

```
#1  Gateway       LiteLLM
#2  Memory        mem0
#3  Knowledge     LlamaIndex
#4  Browser       browser-use
#5  Tools         Your framework (built-in)
```

```bash
pip install litellm mem0 llama-index browser-use
```

5 tools. 90% of use cases. All open source. All security-audited.

> 📖 Full documentation is in Chinese for now. English docs coming soon.

---

<div align="center">

**Built with ❤️ by developers, for developers**

*We already picked for you. You just install.*

</div>
