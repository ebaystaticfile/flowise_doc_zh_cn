---
description: 学习如何上传图像、音频和其他文件
---

# 文件上传

Flowise 允许您从聊天中上传图像、音频和其他文件。在本节中，您将学习如何启用和使用这些功能。

## 图片

某些聊天模型允许您输入图像。请始终参考 LLM 的官方文档以确认模型是否支持图像输入。

* [ChatOpenAI](../integrations/llamaindex/chat-models/chatopenai_zh.md)
* [AzureChatOpenAI](../integrations/llamaindex/chat-models/azurechatopenai_zh.md)
* [ChatAnthropic](../integrations/langchain/chat-models/chatanthropic_zh.md)
* [AWSChatBedrock](../integrations/langchain/chat-models/aws-chatbedrock_zh.md)
* [ChatGoogleGenerativeAI](../integrations/langchain/chat-models/google-ai_zh.md)
* [ChatOllama](../integrations/llamaindex/chat-models/chatollama_zh.md)
* [Google Vertex AI](../integrations/langchain/llms/googlevertex-ai_zh.md)

{% hint style="warning" %}
图像处理仅适用于 Chatflow 中的某些链/代理。

[LLMChain](../integrations/langchain/chains/llm-chain_zh.md), [Conversation Chain](../integrations/langchain/chains/conversation-chain_zh.md), [ReAct Agent](../integrations/langchain/agents/react-agent-chat_zh.md), [Conversational Agent](../integrations/langchain/agents/conversational-agent_zh.md), [Tool Agent](../integrations/langchain/agents/tool-agent_zh.md)
{% endhint %}

如果您启用**允许图像上传**，则可以从聊天界面上传图像。

<div align="center"><figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="255"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-02-29 011714.png" alt="" width="290"><figcaption></figcaption></figure></div>

要使用 API 上传图像：

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "你能描述一下这张图片吗？",
    "uploads": [
        {
            "data": "data:image/png;base64,iVBORw0KGgdM2uN0", # base64 字符串或 URL
            "type": "file", # file | url
            "name": "Flowise.png",
            "mime": "image/png"
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "你能描述一下这张图片吗？",
    "uploads": [
        {
            "data": "data:image/png;base64,iVBORw0KGgdM2uN0", // base64 字符串或 URL
            "type": "file", // file | url
            "name": "Flowise.png",
            "mime": "image/png"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## 音频

在 Chatflow 配置中，您可以选择一个语音转文本模块。支持的集成包括：

* OpenAI
* AssemblyAI
* [LocalAI](../integrations/langchain/chat-models/chatlocalai_zh.md)

启用此功能后，用户可以直接对着麦克风说话。他们的语音将被转录成文本。

<div align="left"><figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-02-29 012538.png" alt="" width="431"><figcaption></figcaption></figure></div>

要使用 API 上传音频：

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "uploads": [
        {
            "data": "data:audio/webm;codecs=opus;base64,GkXf", # base64 字符串
            "type": "audio",
            "name": "audio.wav",
            "mime": "audio/webm"
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "uploads": [
        {
            "data": "data:audio/webm;codecs=opus;base64,GkXf", // base64 字符串
            "type": "audio",
            "name": "audio.wav",
            "mime": "audio/webm"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## 文件

您可以通过两种方式上传文件：

* 检索增强生成 (RAG) 文件上传
* 全文件上传

当两个选项都开启时，全文件上传优先。

### RAG 文件上传

您可以动态地将上传的文件更新到向量数据库。要启用文件上传，请确保您满足以下先决条件：

* 您必须在 chatflow 中包含一个支持文件上传的向量数据库。
  * [Pinecone](../integrations/langchain/vector-stores/pinecone_zh.md)
  * [Milvus](../integrations/langchain/vector-stores/milvus_zh.md)
  * [Postgres](../integrations/langchain/vector-stores/postgres_zh.md)
  * [Qdrant](../integrations/langchain/vector-stores/qdrant_zh.md)
  * [Upstash](../integrations/langchain/vector-stores/upstash-vector_zh.md)
* 如果您的 chatflow 中有多个向量数据库，您一次只能为一个向量数据库启用文件上传。
* 您必须将至少一个文档加载器节点连接到向量数据库的文档输入。
* 支持的文档加载器：
  * [CSV 文件](../integrations/langchain/document-loaders/csv-file_zh.md)
  * [Docx 文件](../integrations/langchain/document-loaders/docx-file_zh.md)
  * [Json 文件](../integrations/langchain/document-loaders/json-file_zh.md)
  * [Json Lines 文件](../integrations/langchain/document-loaders/json-lines-file_zh.md)
  * [PDF 文件](../integrations/langchain/document-loaders/pdf-file_zh.md)
  * [文本文件](../integrations/langchain/document-loaders/text-file_zh.md)
  * [非结构化文件](../integrations/langchain/document-loaders/unstructured-file-loader_zh.md)

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

您可以在聊天中上传一个或多个文件：

<div align="left"><figure><img src="../.gitbook/assets/image (3) (1) (1) (1).png" alt="" width="380"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-08-26 170456.png" alt=""><figcaption></figcaption></figure></div>

工作原理如下：

1. 上传文件的元数据将使用 chatId 更新。
2. 这会将文件与 chatId 关联起来。
3. 查询时，将应用**OR**过滤器：

* 元数据包含`flowise_chatId`，其值为当前聊天会话 ID
* 元数据不包含`flowise_chatId`

在 Pinecone 上更新的向量嵌入示例：

<figure><img src="../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

要使用 API 执行此操作，请按照以下两个步骤操作：

1. 使用带有`formData`和`chatId`的[向量更新 API](api.md#vector-upsert-api)：

{% tabs %}
{% tab title="Python" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowid>"

# 使用表单数据上传文件
form_data = {
    "files": ("state_of_the_union.txt", open("state_of_the_union.txt", "rb"))
}

body_data = {
    "chatId": "some-session-id"
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
// 使用 FormData 上传文件
let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("chatId", "some-session-id");

async function query(formData) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatflowid>",
        {
            method: "POST",
            body: formData
        }
    );
    const result = await response.json();
    return result;
}

query(formData).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

2. 使用带有`uploads`和步骤 1 中的`chatId`的[预测 API](api.md#prediction)：

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "这篇演讲是关于什么的？",
    "chatId": "same-session-id-from-step-1",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:rag",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "这篇演讲是关于什么的？",
    "chatId": "same-session-id-from-step-1",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:rag",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### 全文件上传

使用 RAG 文件上传，您无法处理结构化数据（如电子表格或表格），并且由于缺乏完整上下文，您无法执行完整的摘要。在某些情况下，您可能希望将所有文件内容直接包含在 LLM 的提示中，尤其是在 Gemini 和 Claude 等具有更长上下文窗口的模型中。[这篇研究论文](https://arxiv.org/html/2407.16833v1) 是比较 RAG 与更长上下文窗口的众多论文之一。

要启用全文件上传，请转到**Chatflow 配置**，打开**文件上传**选项卡，然后单击开关：

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

您可以在聊天中看到**文件附件**按钮，您可以在其中上传一个或多个文件。在后台，[文件加载器](../integrations/langchain/document-loaders/file-loader_zh.md) 将处理每个文件并将其转换为文本。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

要使用 API 上传文件：

{% tabs %}
{% tab title="Python" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowid>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "数据是关于什么的？",
    "chatId": "some-session-id",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:full",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
})
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowid>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "数据是关于什么的？",
    "chatId": "some-session-id",
    "uploads": [
        {
            "data": "data:text/plain;base64,TWFkYWwcy4=",
            "type": "file:full",
            "name": "state_of_the_union.txt",
            "mime": "text/plain"
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

如示例所示，上传需要 base64 字符串。要获取文件的 base64 字符串，请使用[创建附件 API](../api-reference/attachments_zh.md)。

### 全文件上传与 RAG 上传的区别

全文件上传和 RAG（检索增强生成）文件上传都服务于不同的目的。

* **全文件上传**: 此方法将整个文件解析为字符串并将其发送到 LLM（大型语言模型）。这对于总结文档或提取关键信息非常有用。但是，对于非常大的文件，由于标记限制，模型可能会产生不准确的结果或“幻觉”。
* **RAG 文件上传**: 如果您旨在通过不将整个文本发送到 LLM 来降低标记成本，则建议使用此方法。这种方法适用于文档上的问答任务，但不适合摘要，因为它缺乏完整的文档上下文。由于需要更新过程，此方法可能需要更长的时间。
