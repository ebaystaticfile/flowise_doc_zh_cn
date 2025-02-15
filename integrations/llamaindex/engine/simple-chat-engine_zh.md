# 简单聊天引擎(Simple Chat Engine)

一个简单的聊天引擎(Simple Chat Engine)充当了AI与用户之间进行对话的完整管道，但不包含上下文检索功能。然而，它配备了[记忆 Memory](../../langchain/memory/)，能够记住对话内容。

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

## 输入(Inputs)

* 聊天模型(Chat Model)
* [记忆 Memory](../../langchain/memory/)

## 参数(Parameters)

| 名称(Name)           | 描述(Description)                                   |
| -------------- | --------------------------------------------- |
| 系统消息(System Message) | 用于指导LLM如何回答查询的指令 |

## 输出(Outputs)

| 名称(Name)             | 描述(Description)                   |
| ---------------- | ----------------------------- |
| 简单聊天引擎(SimpleChatEngine) | 返回响应的最终节点 |