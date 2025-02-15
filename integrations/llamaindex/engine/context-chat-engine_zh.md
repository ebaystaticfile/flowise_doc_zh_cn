# 上下文聊天引擎(Context Chat Engine)

聊天引擎(chat engine)是一个端到端的管道，用于与您的数据进行类似人类的对话，允许多次交流，而不仅仅是单一的问答交互。

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 输入(Inputs)

* 聊天模型(Chat Model)
* 向量存储检索器(Vector Store Retriever)
* [记忆 Memory](../../langchain/memory/)

## 参数(Parameters)

| 名称(Name)                    | 描述(Description)                                                         |
| ----------------------- | ------------------------------------------------------------------- |
| 返回源文档(Return Source Documents) | 返回用于构建响应的引用/来源(citations/sources) |
| 系统消息(System Message)          | 对LLM（大语言模型）如何回答查询的指令                       |

## 输出(Outputs)

| 名称(Name)              | 描述(Description)                   |
| ----------------- | ----------------------------- |
| 上下文聊天引擎(ContextChatEngine) | 返回响应的最终节点 |