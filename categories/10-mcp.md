# MCP 生态

> Model Context Protocol - Agent 工具连接的新兴标准。

---

## 什么是 MCP？

Anthropic 提出的开放协议，标准化 LLM 与外部工具的连接方式。写一次 MCP Server，所有支持 MCP 的 Agent 都能用。

## 推荐方案

### awesome-mcp-servers (88k Star)

索引，不是推荐。里面几千个服务器，质量参差不齐。注意筛选。

### 官方 MCP SDK

TypeScript + Python SDK，自己写 MCP Server 用这个。

### 常用高质量服务器

**官方 (放心用):**
- filesystem / github / postgres / puppeteer / brave-search

**社区验证 (可以用):**
- mcp-server-memory / mcp-server-sqlite / mcp-server-fetch

## MCP 安全分级

```
Green  官方认证   Anthropic/大厂出品 -> 放心用
Yellow 社区验证   100+ Star, 活跃维护 -> 可以用
Orange 社区早期   新项目 -> 沙箱测试
Red    未验证     无人使用/可疑 -> 不要碰
```

## 安全提醒

MCP 服务器直接暴露给 LLM，风险特别高:
1. Prompt 注入 - 恶意服务器可操纵 LLM 行为
2. 数据泄露 - 服务器可以读取你的文件
3. 供应链攻击 - 依赖被篡改
4. 权限提升 - 服务器可能请求不必要的权限
