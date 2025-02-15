# 查询引擎工具(Query Engine Tool)

将查询引擎(Query Engine)转换为工具(Tool)，以便[子问题查询引擎(Sub-Question Query Engine)](../engine/sub-question-query-engine_zh.md)或代理(Agent)使用。

<figure><img src="../../../.gitbook/assets/image (9) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

## 输入(Inputs)

* 向量存储索引(Vector Store Index)

## 参数(Parameters)

| 名称(Tool Name)             | 描述(Description)                                         |
| ---------------- | --------------------------------------------------- |
| 工具名称(Tool Name)        | 工具的名称                                    |
| 工具描述(Tool Description) | 描述LLM何时应使用此工具 |

## 输出(Outputs)

| 名称(Name)            | 描述(Description)                                                                                      |
| --------------- | ------------------------------------------------------------------------------------------------ |
| 查询引擎工具(QueryEngineTool) | 连接到代理(Agent)或[子问题查询引擎(Sub-Question Query Engine)](../engine/sub-question-query-engine_zh.md)的接口 |