# 小团队（3-5人）

> 团队协作，工具要统一。

---

## 你们的需求

- 统一的 API 管理（多人共用）
- 成本分摊和监控
- 共享知识库
- 工作流自动化

## 推荐配置

### 必装

```bash
pip install litellm mem0 llama-index
```

- **LiteLLM Proxy Server** - 团队共享 API 网关
- **mem0** - 共享团队记忆
- **LlamaIndex** - 团队知识库

### 加装

- **LangGraph** - 复杂工作流编排
- **LangSmith** - 团队级可观测性

### 预算

- 月费: $100-300（多人使用）

## 架构

```
所有人 -> LiteLLM Proxy -> 各 LLM 厂商
         |
         +-> mem0 (共享记忆)
         +-> LlamaIndex (共享知识库)
         +-> LangSmith (监控)
```
