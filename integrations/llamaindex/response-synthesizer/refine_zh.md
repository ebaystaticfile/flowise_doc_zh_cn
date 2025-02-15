# 精炼(Refine)

通过依次处理每个检索到的文本块来创建和完善答案。

**优点**：适合生成更详细的答案

**缺点**：每个节点(Node)需要单独的LLM调用（可能成本较高）

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

**精炼提示(Refine Prompt)**

```markup
The original query is as follows: {query}
We have provided an existing answer: {existingAnswer}
We have the opportunity to refine the existing answer (only if needed) with some more context below.
------------
{context}
------------
Given the new context, refine the original answer to better answer the query. If the context isn't useful, return the original answer.
Refined Answer:
```

**文本问答提示(Text QA Prompt)**

```
Context information is below.
---------------------
{context}
---------------------
Given the context information and not prior knowledge, answer the query.
Query: {query}
Answer:
```