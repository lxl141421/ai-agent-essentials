# LLM 网关 / Provider 管理

> 统一管理多个 LLM API，一套代码切换任意厂商。

---

## 为什么需要网关？

- LLM 厂商 API 格式各不相同，换厂商要改代码
- 不同模型价格差异 100 倍，需要智能路由
- 单一厂商可能宕机，需要 fallback
- 需要统一监控 token 消耗和成本

## 推荐方案

### 🥇 LiteLLM

| 项目 | 详情 |
|------|------|
| GitHub | [BerriAI/litellm](https://github.com/BerriAI/litellm) |
| Star | ~49k |
| 协议 | MIT |
| 语言 | Python |

**为什么选它：**
- 支持 100+ LLM API（OpenAI、Anthropic、Google、DeepSeek、阿里、字节……）
- OpenAI 格式兼容——你的代码只需要写一次
- 内置成本追踪、负载均衡、fallback
- 可以当 Proxy Server 部署，团队共享

**快速开始：**
```python
from litellm import completion

# 统一接口，切换厂商只改 model 名
response = completion(
    model="gpt-4o",           # OpenAI
    # model="claude-sonnet-4",  # Anthropic
    # model="qwen-max",         # 阿里
    messages=[{"role": "user", "content": "Hello!"}]
)
```

**安全评估：**
- 协议：✅ MIT
- 数据：⚠️ 可配置（默认本地，可选云端追踪）
- 审查：✅ 大量用户使用，代码公开

---

### 🥈 OpenRouter

| 项目 | 详情 |
|------|------|
| 官网 | [openrouter.ai](https://openrouter.ai/) |
| 协议 | 商业服务 |
| 付费 | 按量付费 |

**为什么选它：**
- 一个 API Key 访问所有模型
- 自动 fallback（某厂商挂了自动切换）
- 有免费模型可以白嫖
- 不需要自己部署

**适合场景：** 不想折腾部署，直接用。

**安全评估：**
- 数据：☁️ 数据经过 OpenRouter 服务器
- 审查：⚠️ 商业服务，需信任第三方

---

### 🥉 Hermes Agent 内置

| 项目 | 详情 |
|------|------|
| GitHub | [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) |
| 协议 | Apache 2.0 |

**为什么选它：**
- 内置 20+ Provider 支持
- Credential Pool 自动轮转多个 API Key
- 不需要额外部署网关

**适合场景：** 已经在用 Hermes Agent 的用户。

---

## 决策树

```
你想怎么管理 LLM API？
│
├── 我想一行代码切换所有厂商
│   └── → LiteLLM（本地部署）
│
├── 我不想折腾，直接用
│   └── → OpenRouter（云端服务）
│
├── 我已经在用 Agent 框架
│   └── → 框架内置的 Provider 管理
│
└── 我是大团队，需要统一管控
    └── → LiteLLM Proxy Server + W&B 监控
```

---

*每个工具都经过实际使用验证。如有更新，欢迎 PR！*
