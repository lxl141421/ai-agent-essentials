# 上下文管理

> 窗口就那么大，怎么塞最多有效信息？

---

## 核心问题

LLM 的上下文窗口是有限的（4K ~ 200K tokens），但 Agent 需要处理的信息可能远超这个限制：
- 系统 Prompt
- 用户历史对话
- 工具调用结果（可能很长）
- 知识库检索结果
- 文件内容

**不管理上下文 = 要么丢失信息，要么爆 token 费用。**

## 推荐方案

### 🥇 滑动窗口 + 摘要压缩

**策略：** 最近的对话保留原文，更早的对话压缩成摘要。

**优点：** 简单可靠，大多数 Agent 框架内置支持。
**缺点：** 压缩会丢失细节。

```
对话历史：
  [消息1] ← 压缩为摘要
  [消息2] ← 压缩为摘要
  [消息3] ← 压缩为摘要
  [消息4] ← 保留原文
  [消息5] ← 保留原文
  [当前]  ← 正在处理
```

**实现：**
- Hermes Agent：内置 `compressor` 引擎，自动触发
- LangChain：`ConversationSummaryBufferMemory`
- LlamaIndex：`ChatSummaryMemoryBuffer`

---

### 🥈 LLMLingua

| 项目 | 详情 |
|------|------|
| GitHub | [microsoft/LLMLingua](https://github.com/microsoft/LLMLingua) |
| Star | ~8k |
| 协议 | MIT |
| 出品 | 微软研究院 |

**为什么选它：**
- 用小模型（如 GPT-2）判断哪些 token 可以删除
- 可以压缩 2-10 倍，几乎不损失语义
- 有 LLMLingua-2 版本，速度更快

**快速开始：**
```python
from llmlingua import PromptCompressor

compressor = PromptCompressor(
    model_name="microsoft/llmlingua-2-xlm-roberta-large-meetingbank",
    device_map="cpu"
)

result = compressor.compress_prompt(
    your_long_prompt,
    rate=0.5,  # 压缩到 50%
    force_tokens=['\n', '?', '!', '.']  # 保留这些 token
)
print(f"压缩率: {result['rate']}")
```

**安全评估：**
- 协议：✅ MIT
- 数据：🔒 本地运行
- 审查：✅ 微软出品

---

### 🥉 分层上下文

**策略：** 按优先级分层，超出窗口时从低优先级开始丢弃。

```
优先级（高→低）：
  1. 系统 Prompt（永远保留）
  2. 当前任务上下文
  3. 最近 3 轮对话
  4. 工具调用结果（只保留摘要）
  5. 更早的对话历史
```

**适合场景：** 工具调用结果很长的 Agent（如浏览器截图、终端输出）。

---

## 决策树

```
你的上下文管理需求？
│
├── 最简单，能用就行
│   └── → 滑动窗口 + 摘要（框架内置）
│
├── Token 预算很紧，要极致压缩
│   └── → LLMLingua（微软出品，2-10x 压缩）
│
├── 工具输出特别长（浏览器/终端）
│   └── → 分层上下文（优先级策略）
│
└── 我用的是 Agent 框架
    └── → 先看框架自带什么，一般够用
```

---

*每个工具都经过实际使用验证。如有更新，欢迎 PR！*
