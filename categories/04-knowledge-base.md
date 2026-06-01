# 知识库 / RAG

> 让 Agent 能查阅你的文档、代码库、私有数据。

---

## 什么是 RAG？

Retrieval-Augmented Generation（检索增强生成）：
1. 把你的文档切块，转成向量存起来
2. 用户提问时，先检索相关文档片段
3. 把检索结果和问题一起发给 LLM

**好处：** LLM 不用记住所有内容，按需查阅就行。

## 推荐方案

### 🥇 LlamaIndex

| 项目 | 详情 |
|------|------|
| GitHub | [run-llama/llama_index](https://github.com/run-llama/llama_index) |
| Star | ~50k |
| 协议 | MIT |
| 语言 | Python |

**为什么选它：**
- RAG 领域的标杆项目
- 160+ 数据连接器（PDF、Notion、GitHub、数据库……）
- 索引策略丰富（向量、关键词、知识图谱）
- 与 LlamaHub 生态深度集成

**快速开始：**
```python
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader

# 加载文档
documents = SimpleDirectoryReader("./docs").load_data()

# 建索引
index = VectorStoreIndex.from_documents(documents)

# 查询
query_engine = index.as_query_engine()
response = query_engine.query("这个项目的架构是什么？")
print(response)
```

**安全评估：**
- 协议：✅ MIT
- 数据：🔒 默认本地（可选云端向量库）
- 审查：✅ 50k Star，社区活跃

---

### 🥈 LangChain Retrieval

| 项目 | 详情 |
|------|------|
| GitHub | [langchain-ai/langchain](https://github.com/langchain-ai/langchain) |
| Star | ~138k |
| 协议 | MIT |

**为什么选它：**
- 如果已经在用 LangChain，不需要引入额外依赖
- 检索链、对话链、工具链无缝集成
- 社区最大，文档最全

**适合场景：** 已经在 LangChain 生态里的项目。

**安全评估：**
- 协议：✅ MIT
- 数据：⚠️ 可配置（取决于用哪个向量库）
- 审查：✅ 138k Star

---

### 🥉 轻量方案：ChromaDB / FAISS

**适合场景：** 数据量小（< 1000 页），不想引入大框架。

```python
# ChromaDB - 最简单的向量数据库
import chromadb

client = chromadb.Client()
collection = client.create_collection("my_docs")
collection.add(
    documents=["文档内容1", "文档内容2"],
    ids=["doc1", "doc2"]
)
results = collection.query(query_texts=["搜索词"], n_results=2)
```

**安全评估：**
- 协议：✅ Apache 2.0（ChromaDB）/ MIT（FAISS）
- 数据：🔒 完全本地

---

## 向量数据库对比

| 数据库 | 类型 | 适合规模 | 协议 |
|--------|------|---------|------|
| ChromaDB | 嵌入式 | 小型 | Apache 2.0 |
| FAISS | 库 | 中型 | MIT |
| Milvus | 分布式 | 大型 | Apache 2.0 |
| Weaviate | 服务 | 中大型 | BSD-3 |
| Pinecone | 云服务 | 任意 | 商业 |

---

## 决策树

```
你的数据量有多大？
│
├── < 1000 页文档
│   └── → ChromaDB（嵌入式，零配置）
│
├── 1000 ~ 10000 页
│   └── → LlamaIndex + ChromaDB/Milvus
│
├── > 10000 页 或 多数据源
│   └── → LlamaIndex + Milvus/Weaviate
│
└── 我已经在用 LangChain
    └── → LangChain Retrieval（不引入新依赖）
```

---

*每个工具都经过实际使用验证。如有更新，欢迎 PR！*
