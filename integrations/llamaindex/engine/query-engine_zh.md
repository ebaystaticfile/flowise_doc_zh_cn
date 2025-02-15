# 查询引擎(Query Engine)

查询引擎(Query Engine)作为一个端到端的管道，使用户能够对其数据提出问题。它接收自然语言查询并提供响应，同时附带检索到的相关上下文信息并传递给LLM（大语言模型，Large Language Model）。

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

## 输入(Inputs)

* 向量存储检索器(Vector Store Retriever)
* [响应合成器 Response Synthesizer](../response-synthesizer/)

## 参数(Parameters)

| 名称(Name)                    | 描述(Description)                                                         |
| ----------------------- | ------------------------------------------------------------------- |
| 返回源文档(Return Source Documents) | 返回用于构建响应的引用/来源(citations/sources) |

## 输出(Outputs)

| 名称(Name)        | 描述(Description)                   |
| ----------- | ----------------------------- |
| 查询引擎(QueryEngine) | 返回响应的最终节点(Final node to return response) |