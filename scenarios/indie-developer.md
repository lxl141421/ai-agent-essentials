# 独立开发者（全栈）

> 一个人扛所有，工具必须省心。

---

## 你的需求

- 能帮我写代码、改 Bug
- 能帮我查文档、搜 Stack Overflow
- 能帮我写文档、整理笔记
- 预算有限，要省 Token

## 推荐配置

### 必装（Day 1）

```bash
pip install litellm mem0
```

- **LiteLLM** - 统一 API 管理，一个 Key 用所有模型
- **mem0** - 记住你的技术栈和偏好
- **终端 + 文件工具** - 框架自带

### 加装（Week 1）

```bash
pip install browser-use llama-index
```

- **browser-use** - 自动查阅在线文档
- **LlamaIndex** - 索引你的代码库

### 预算

- 月费: $20-50（取决于使用频率）
- 省钱技巧: 简单任务用 DeepSeek / Qwen，复杂任务才用 GPT-4o

## 典型工作流

```
1. "帮我看看这个 Bug" -> 终端跑测试 + 读代码
2. "这个 API 怎么用" -> browser-use 查文档
3. "帮我写个 README" -> 读代码库 + 生成文档
4. "上次那个方案是什么" -> mem0 检索记忆
```

## 注意事项

- API Key 放 .env，不要硬编码
- 简单问题别用最贵的模型
- 工具输出截断，省 token
