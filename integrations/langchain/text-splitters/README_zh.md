---
description: LangChain 文本分割器节点
---

# 文本分割器

***

**当您需要处理长文本时，有必要将文本分割成小块。**\
虽然听起来很简单，但这其中可能涉及很多复杂性。理想情况下，您希望将语义相关的文本片段保持在一起。而“语义相关”的含义可能取决于文本的类型。本笔记本展示了实现这一目标的几种方法。

**从高层次来看，文本分割器的工作方式如下：**

1. 将文本分割成小的、语义上有意义的块（通常是句子）。
2. 开始将这些小块组合成较大的块，直到达到某个大小（通过某种函数测量）。
3. 一旦达到该大小，将该块作为独立的文本片段，然后开始创建一个新的文本块，并保留一些重叠部分（以保持块之间的上下文）。

**这意味着您可以通过两种不同的方式自定义文本分割器：**

1. 文本如何分割
2. 块大小的测量方式

### 文本分割器节点：

* [字符文本分割器 Character Text Splitter](character-text-splitter_zh.md)
* [代码文本分割器 Code Text Splitter](code-text-splitter_zh.md)
* [HTML 转 Markdown 文本分割器 Html-To-Markdown Text Splitter](html-to-markdown-text-splitter_zh.md)
* [Markdown 文本分割器 Markdown Text Splitter](markdown-text-splitter_zh.md)
* [递归字符文本分割器 Recursive Character Text Splitter](recursive-character-text-splitter_zh.md)
* [令牌文本分割器 Token Text Splitter](token-text-splitter_zh.md)