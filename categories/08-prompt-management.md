# Prompt 管理

> Prompt 就是代码，也要版本控制。

---

## 推荐方案

### Git 版本控制 (首选)

最朴素也最靠谱。Prompt 文件放 Git 仓库，改动就是一次 commit。

```
prompts/
  system.md
  tools/search.md
  templates/review.md
```

### 框架内置模板

LangChain 的 PromptTemplate, LlamaIndex 的 PromptMixin, Hermes Agent 的 personalities。

### PromptLayer / Helicone

云服务，适合团队协作和 A/B 测试。注意: 你的 Prompt 会经过第三方服务器。

## 最佳实践

1. Prompt 放版本控制
2. 模板化，变量用占位符
3. 改了 Prompt 要跑测试用例
4. 每个 Prompt 说明用途和预期输出
5. 大改之前先 A/B 测试
