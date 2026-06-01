# 浏览器自动化

> 让 Agent 能上网、操作网页、填表单、提取数据。

---

## 为什么需要浏览器自动化？

- Agent 需要查阅在线文档和 API 参考
- 操作 Web 应用（填表、点击、提交）
- 抓取动态渲染的网页内容
- 测试 Web 应用

## 技术栈对比

| 方案 | 底层 | 上手难度 | 灵活性 | 适合场景 |
|------|------|---------|--------|---------|
| browser-use | Playwright | ⭐ 简单 | ⭐⭐⭐ | AI Agent 自动化 |
| Nanobrowser | Chrome 扩展 | ⭐⭐ 中等 | ⭐⭐ | 浏览器内 Agent |
| CDP 直连 | Chrome DevTools | ⭐⭐⭐ 复杂 | ⭐⭐⭐⭐ | 底层控制 |

## 推荐方案

### 🥇 browser-use

| 项目 | 详情 |
|------|------|
| GitHub | [browser-use/browser-use](https://github.com/browser-use/browser-use) |
| Star | ~96k |
| 协议 | MIT |
| 底层 | Playwright |

**为什么选它：**
- 当前最火的 AI 浏览器自动化项目
- 专门为 LLM Agent 设计
- 开箱即用，API 简洁
- 支持多标签、iframe、文件上传

**快速开始：**
```python
from browser_use import Agent

agent = Agent(
    task="去 Google 搜索 'AI Agent 2026'，返回前 3 个结果的标题",
    llm=your_llm
)
result = await agent.run()
```

**安全评估：**
- 协议：✅ MIT
- 数据：🔒 本地浏览器操作
- 审查：✅ 96k Star，社区活跃

---

### 🥈 Nanobrowser

| 项目 | 详情 |
|------|------|
| GitHub | [nanobrowser/nanobrowser](https://github.com/nanobrowser/nanobrowser) |
| Star | ~13k |
| 协议 | MIT |

**为什么选它：**
- Chrome 扩展形态，不需要额外进程
- 支持多 Agent 协作浏览
- 可以复用已有的 Chrome 登录态
- OpenAI Operator 的开源替代

**安全评估：**
- 协议：✅ MIT
- 数据：🔒 本地操作
- 审查：⚠️ 相对较新，自行审查代码

---

### 🥉 CDP 直连

Chrome DevTools Protocol —— 最底层的浏览器控制。

**为什么选它：**
- 无依赖，直接通过 WebSocket 控制 Chrome
- 完全控制浏览器行为
- 适合定制化需求

**快速开始：**
```bash
# 启动 Chrome 开启调试端口
google-chrome --remote-debugging-port=9222

# 通过 CDP WebSocket 控制
curl http://localhost:9222/json
```

**安全评估：**
- 协议：✅ 无第三方依赖
- 数据：🔒 完全本地
- 审查：✅ 协议本身是 Chrome 标准功能

---

## 决策树

```
你需要什么级别的浏览器控制？
│
├── Agent 自动完成网页任务
│   └── → browser-use（最简单、最成熟）
│
├── 需要复用已有登录态
│   └── → Nanobrowser（Chrome 扩展）
│
├── 需要最底层控制
│   └── → CDP 直连
│
└── 只需要抓取静态网页
    └── → 不需要浏览器！用 requests / curl 就够了
```

---

*每个工具都经过实际使用验证。如有更新，欢迎 PR！*
