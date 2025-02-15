---
description: LlamaIndex 嵌入节点
---

# 嵌入

***

嵌入（Embedding）是一个浮点数组成的向量（列表）。两个向量之间的距离衡量了它们的相关性。距离越小，相关性越高；距离越大，相关性越低。

嵌入可用于创建文本数据的数值表示。这种数值表示非常有用，因为它可以用来查找相似的文档。

它们通常用于以下场景：

* 搜索（根据与查询字符串的相关性对结果进行排序）
* 聚类（根据相似性对文本字符串进行分组）
* 推荐（推荐具有相关文本字符串的项目）
* 异常检测（识别相关性较低的异常值）
* 多样性测量（分析相似性分布）
* 分类（根据最相似的标签对文本字符串进行分类）

### 嵌入节点：

* [Azure OpenAI 嵌入 Azure OpenAI Embeddings](azure-openai-embeddings_zh.md)
* [OpenAI 嵌入 OpenAI Embedding](openai-embedding_zh.md)