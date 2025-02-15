# 子问题查询引擎(Sub-Question Query Engine)

一个设计用于解决使用多个数据源回答复杂查询问题的查询引擎。它首先将复杂查询分解为每个相关数据源的子问题，然后收集所有中间响应并合成最终响应。

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

## 输入(Inputs)

* 查询引擎工具(Query Engine Tools)
* 聊天模型(Chat Model)
* 嵌入(Embeddings)
* [响应合成器 Response Synthesizer](../response-synthesizer/)

## 参数(Parameters)

| 名称(Name)                    | 描述(Description)                                                         |
| ----------------------- | ------------------------------------------------------------------- |
| 返回源文档(Return Source Documents) | 返回用于构建响应的引用/来源(To return citations/sources that were used to build up the response) |

## 输出(Outputs)

| 名称(Name)                   | 描述(Description)                   |
| ---------------------- | ----------------------------- |
| 子问题查询引擎(SubQuestionQueryEngine) | 返回响应的最终节点(Final node to return response) |