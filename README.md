# AI Agent Essentials

**不是又一个 Awesome List。**

我花了几个月折腾各种 Agent 框架和工具，踩了无数坑，才搞明白一件事：**你不需要 8000 个选项，你需要有人告诉你该用哪几个。**

这个仓库记录的就是这些经验。每个领域我只推荐 2-3 个工具，附带我踩过的坑和安全方面的考量。不多，但够用。

> awesome-mcp-servers 列了 8000 个，awesome-llm-apps 列了 100+ 个。
> 你只需要 5 个。

---

## 先说结论：5 个工具搞定 90% 场景

```
#1  LLM 网关     →  LiteLLM
#2  记忆系统     →  mem0
#3  知识库       →  LlamaIndex
#4  浏览器       →  browser-use
#5  工具系统     →  你的框架自带
```

```bash
pip install litellm mem0 llama-index browser-use
```

装完就能跑。不需要再纠结了。

---

## 为什么要做这个？

市面上的 Awesome Lists 有一个共同问题：**它们把选择的负担推给了你。**

列 100 个项目很容易，但对新手来说，100 个选项等于没有选项。你需要的不是更多信息，而是有人帮你做决定。

这个仓库就是那个帮你做决定的人。

每个领域我会告诉你：
- 用什么（2-3 个选择，不是 20 个）
- 为什么选它（真实的使用体验，不是复制官方 README）
- 有什么坑（我踩过的，你不用再踩）
- 安全上要注意什么（MCP 服务器有没有后门？数据去了哪里？）

---

## 分类

我把 Agent 基础设施分成四层，按重要性排：

### 第一层：没有就跑不起来

**LLM 网关** — 你需要一个统一的方式调用各种 LLM API。我推荐 [LiteLLM](https://github.com/BerriAI/litellm)，100+ 厂商一个接口搞定。不想自己部署就用 [OpenRouter](https://openrouter.ai/)，按量付费。

**工具系统** — 终端执行、文件读写。这个框架自带，不需要额外装东西。

**上下文管理** — 窗口就那么大，怎么塞更多信息？大多数框架内置的滑动窗口 + 摘要压缩就够用了。Token 特别紧张的话看看 [LLMLingua](https://github.com/microsoft/LLMLingua)，微软出的，能压缩 2-10 倍。

### 第二层：Agent 的核心竞争力

**记忆系统** — [mem0](https://github.com/mem0ai/mem0)（57k Star）。让 Agent 跨会话记住你是谁、你的偏好。不过注意：默认走云端，敏感数据要配置成本地模式。

**知识库** — [LlamaIndex](https://github.com/run-llama/llama_index)（50k Star）。让你的 Agent 能查阅自己的文档和代码库。数据量小的话 ChromaDB 够了，不用上大框架。

**浏览器** — [browser-use](https://github.com/browser-use/browser-use)（96k Star）。让 Agent 能上网、操作网页。需要复用登录态的话看看 Nanobrowser。

### 第三层：省钱省心

**Token 监控** — 用 LiteLLM 的话自带成本追踪，不需要额外工具。想看实时仪表盘可以试试 Tokentap。

**Prompt 管理** — Git。就是 Git。Prompt 文件放仓库里，改了就 commit，最朴素也最靠谱。

**多模型路由** — LiteLLM 内置 fallback，主模型挂了自动切备用的。

### 第四层：锦上添花

**多 Agent** — 大多数任务单 Agent + 好工具就够了。真需要的话 LangGraph 最灵活，MetaGPT 适合模拟团队协作。

**MCP 生态** — 新兴标准，但安全问题没人管。我做了一个分级：官方的放心用，社区的看 Star 和维护频率，没听过的先在沙箱里跑。

**可观测性** — LangSmith（LangChain 生态）、Phoenix（开源）。不是必须的，但出了问题能救命。

每个品类的详细评测在 [categories/](categories/) 目录下。

---

## 安全

这个仓库和其他 Awesome Lists 最大的区别：**每个推荐工具我都做了安全评估。**

不是专业审计，但至少检查了：
- 开源协议
- 数据流向（本地还是上云）
- 有没有已知的安全问题
- MCP 服务器有没有可疑行为

MCP 服务器我做了一个四级分级：

```
🟢  官方认证    Anthropic / 大厂出品，放心用
🟡  社区验证    100+ Star，活跃维护，可以用
🟠  社区早期    新项目，自己先审查
🔴  未验证      没人用过，别碰
```

详见 [security/](security/)。

---

## 踩坑记录

这部分可能是这个仓库最有价值的内容。

我花了很多时间在各种环境配置问题上，这些时间你不需要再花了。

**WSL 用户**（最常踩坑的群体）：

```bash
# localhost 连不上 Windows 的 LM Studio
# 因为 WSL2 是虚拟机，localhost 指向的是 WSL 自己
WIN_IP=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}')
curl http://$WIN_IP:1234/v1/models

# Clash/V2Ray 会劫持 API 请求
unset http_proxy https_proxy HTTP_PROXY HTTPS_PROXY

# API Key 别放 config.yaml，会被 Git 泄露
# 放 ~/.hermes/.env
```

**macOS 用户**：

```bash
# Apple Silicon 编译 llama-cpp-python 失败
CMAKE_ARGS="-DGGML_METAL=on" pip install llama-cpp-python
```

**通用**：

```bash
# context_length 设太大，内存爆了
# 本地模型从 4096 开始，别上来就 32K

# 工具输出太长，token 疯涨
# 截断，只保留关键信息
```

完整的踩坑记录在 [setup/](setup/) 目录下，按平台分了四份。

---

## 场景匹配

你是谁？从哪开始？

**独立开发者** — LiteLLM + mem0 + 终端/文件，月费 $20-50。详见 [scenarios/indie-developer.md](scenarios/indie-developer.md)

**前端工程师** — browser-use + 终端/文件，月费 $10-30。详见 [scenarios/frontend-dev.md](scenarios/frontend-dev.md)

**数据科学家** — LlamaIndex + mem0，月费 $30-80。详见 [scenarios/data-scientist.md](scenarios/data-scientist.md)

**小团队（3-5人）** — LiteLLM Proxy + mem0 + LlamaIndex，月费 $100-300。详见 [scenarios/small-team.md](scenarios/small-team.md)

**本地模型玩家** — Ollama / LM Studio / llama.cpp，月费 $0（电费除外）。详见 [scenarios/local-model.md](scenarios/local-model.md)

---

## 项目结构

```
categories/        11 个领域的详细评测
security/          安全审计报告
setup/             4 个平台的配置指南和踩坑记录
scenarios/         5 种用户画像的推荐方案
```

---

## 贡献

欢迎 PR，但有门槛：

1. 必须附安全评估，不接受纯功能推荐
2. 踩坑记录优先——你踩过的坑就是别人省下的时间
3. 每个领域最多 3 个推荐，新增必须替换一个
4. 推荐的工具你至少用过一周

详见 [CONTRIBUTING.md](CONTRIBUTING.md)

---

## English

Not another Awesome List. A curated, opinionated, security-audited guide to AI Agent infrastructure.

The problem: awesome-ai-agents lists 280 projects. awesome-mcp-servers lists 8000. You don't need more options — you need someone to decide for you.

The solution: each category, 2-3 picks. Every tool security-audited. Real pitfall documentation. Scenario-based recommendations.

Full documentation is in Chinese for now. English docs coming soon.

---

*觉得有用就给个 Star，让更多人不用再做选择题。*
