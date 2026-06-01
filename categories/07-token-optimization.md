# Token 监控与优化

> 省下来的就是赚到的。

---

## 为什么重要？

- GPT-4o: 输入 $2.5/M tokens, 输出 $10/M tokens
- Claude Sonnet: 输入 $3/M tokens, 输出 $15/M tokens
- 一个复杂 Agent 任务可能消耗 50K-200K tokens
- 一天跑几十次，月费轻松破千

## 推荐方案

### LiteLLM 内置成本追踪 (首选)

已经在用 LiteLLM 做网关的话，零额外依赖就能用。自动追踪每次请求的 token 消耗和成本。

### Tokentap (~800 Star, MIT)

实时终端仪表盘，拦截 LLM API 流量可视化 token 消耗。无需改代码。

### tiktoken (手动计算)

```python
import tiktoken
enc = tiktoken.encoding_for_model("gpt-4o")
print(f"Token 数: {len(enc.encode('你的文本'))}")
```

## 省钱技巧

1. 上下文压缩 - LLMLingua 可省 2-10x
2. 选择合适模型 - 简单任务用小模型
3. Fallback 策略 - 主模型挂了自动切便宜的
4. 缓存结果 - 相同问题不重复调用
5. 截断工具输出 - 不要全塞进上下文
6. 批量处理 - 多条消息合并请求
