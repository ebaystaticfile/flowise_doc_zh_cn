---
description: LangChain 记忆节点
---

# 记忆

***

记忆功能允许您与 AI 交互，如同 AI 拥有先前对话的记忆。

_<mark style="color:blue;">用户：你好，我是Bob</mark>_

_<mark style="color:orange;">AI：你好，Bob！很高兴认识你。今天我能如何帮助你？</mark>_

_<mark style="color:blue;">用户：我的名字是什么？</mark>_

_<mark style="color:orange;">AI：你的名字是Bob，正如你之前提到的。</mark>_

在底层，这些对话被存储在数组或数据库中，并作为上下文提供给大型语言模型 (LLM)。例如：

代码块 0
您是一位由 OpenAI 训练的大型语言模型驱动的助手。

无论人类需要帮助解决特定问题，还是只想就特定主题进行对话，您都在这里提供帮助。

当前对话：
{history}
代码块 1

### 记忆节点：

* [缓冲区记忆](buffer-memory_zh.md)
* [缓冲区窗口记忆](buffer-window-memory_zh.md)
* [会话摘要记忆](conversation-summary-memory_zh.md)
* [会话摘要缓冲区记忆](conversation-summary-buffer-memory_zh.md)
* [DynamoDB 聊天记忆](dynamodb-chat-memory_zh.md)
* [MongoDB Atlas 聊天记忆](mongodb-atlas-chat-memory_zh.md)
* [基于 Redis 的聊天记忆](redis-backed-chat-memory_zh.md)
* [基于 Upstash Redis 的聊天记忆](upstash-redis-backed-chat-memory_zh.md)
* [Zep 记忆](zep-memory_zh.md)

## 多用户的独立会话

### UI 和嵌入式聊天

默认情况下，UI 和嵌入式聊天会自动将不同用户的会话分开。这是通过为每次新的交互生成唯一的 **`chatId`** 来实现的。此逻辑由 Flowise 在底层处理。

### 预测 API

您可以通过指定唯一的 **`sessionId`** 来分离多个用户的会话。

1. 对于每个记忆节点，您都应该看到一个输入参数 **`会话 ID`**

<figure><img src="../../../.gitbook/assets/image (76).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Untitled (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

2. 在 `/api/v1/prediction/{your-chatflowid}` POST 请求体中，在 **`overrideConfig`** 中指定 **`sessionId`**

```json
{
    "question": "你好！",
    "overrideConfig": {
        "sessionId": "user1"
    }
}
```

### 消息 API

* GET `/api/v1/chatmessage/{your-chatflowid}`
* DELETE `/api/v1/chatmessage/{your-chatflowid}`

<table><thead><tr><th>查询参数</th><th width="192">类型</th><th>值</th></tr></thead><tbody><tr><td>sessionId</td><td>字符串</td><td></td></tr><tr><td>sort</td><td>枚举</td><td>ASC 或 DESC</td></tr><tr><td>startDate</td><td>字符串</td><td></td></tr><tr><td>endDate</td><td>字符串</td><td></td></tr></tbody></table>

所有会话也可以通过 UI 进行可视化和管理：

<figure><img src="../../../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

对于 OpenAI 助手，将使用 [线程](../agents/openai-assistant/threads_zh.md) 来存储会话。
