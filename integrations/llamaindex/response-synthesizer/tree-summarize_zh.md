# 树形总结(Tree Summarize)

当提供文本块和查询时，递归构建树形结构并返回根节点作为结果。

**优点**：适用于总结任务

**缺点**：在遍历树形结构时，答案的准确性可能会丢失

<figure><img src="../../../.gitbook/assets/image (7) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

**提示**

```
Context information from multiple sources is below.
---------------------
{context}
---------------------
Given the information from multiple sources and not prior knowledge, answer the query.
Query: {query}
Answer:
```