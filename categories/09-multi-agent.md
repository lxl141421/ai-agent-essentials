# 多 Agent 协作

> 一个 Agent 搞不定？那就来一群。

---

## 什么时候需要？

- 任务需要多种专业能力（写代码 + 测试 + 部署）
- 任务可以并行处理
- 需要互相审查和纠错
- 任务太复杂，单 Agent 上下文放不下

## 推荐方案

### LangGraph (33k Star, MIT)

有状态图编排，最灵活的多 Agent 架构。支持 human-in-the-loop，内置持久化。适合复杂工作流。

### MetaGPT (68k Star, MIT)

模拟软件公司: 产品经理、架构师、工程师、QA。从需求到代码全流程自动化。

### 委托模式 (最简单)

不引入框架，主 Agent 委托子任务给子 Agent。Hermes Agent 的 delegate_task, Claude Code 的子进程都支持。

## 决策树

- 独立子任务 -> 委托模式
- 复杂条件分支 -> LangGraph
- 模拟团队协作 -> MetaGPT
- 其实一个 Agent 就够 -> 别过度设计!
