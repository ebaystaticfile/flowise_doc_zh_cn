# 紧凑与优化(Compact And Refine)

当没有显式定义响应合成器(Response Synthesizer)时，这是默认选项。

在每次LLM（大语言模型）调用时，通过将尽可能多的文本块(text chunks)压缩到最大提示大小内来紧凑提示(prompt)。如果在一个提示中无法容纳所有文本块，则通过多个紧凑提示“创建并优化”答案。

**优点**: 与[优化 Refine](refine_zh.md)相同，适用于更详细的答案，并且应该会减少LLM调用次数。

**缺点**: 由于多次LLM调用，可能会比较昂贵。

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

**优化提示(Refine Prompt)**

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