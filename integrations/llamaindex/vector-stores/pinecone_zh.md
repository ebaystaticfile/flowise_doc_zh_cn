---
description: >-
  使用 Pinecone（领先的完全托管的托管向量数据库）嵌入数据并执行相似性搜索查询。
---

# Pinecone

## 先决条件

1. 注册一个 [Pinecone](https://app.pinecone.io/) 账户
2. 点击 **Create index**

<figure><img src="../../../.gitbook/assets/pinecone_1.png" alt=""><figcaption></figcaption></figure>

3. 填写必填字段：
   - **Index Name**，要创建的索引名称。（例如 "flowise-test"）
   - **Dimensions**，要插入索引的向量大小。（例如 1536）

<figure><img src="../../../.gitbook/assets/pinecone_2.png" alt="" width="527"><figcaption></figcaption></figure>

4. 点击 **Create Index**

## 设置

1. 获取/创建你的 **API Key**

<figure><img src="../../../.gitbook/assets/pinecone_3.png" alt=""><figcaption></figcaption></figure>

2. 在画布上添加一个新的 **Pinecone** 节点并填写参数：
    - Pinecone Index
    - Pinecone namespace（可选）

<figure><img src="../../../.gitbook/assets/pinecone_llamaindex.png" alt="" width="301"><figcaption><p>Pinecone 节点</p></figcaption></figure>

3. 创建新的 Pinecone 凭证 -> 填写 **API Key**

<figure><img src="../../../.gitbook/assets/pinecone_5.png" alt="" width="563"><figcaption></figcaption></figure>

4. 在画布上添加其他节点并开始 upsert 过程
   - **Document** 可以与 [**Document Loader**](../../langchain/document-loaders/) 类别下的任何节点连接
     {% hint style="info" %}
     LlamaIndex 的文档加载器和文本分割器尚不可用，但使用 LangChain 下可用的加载器仍然可以正常使用 LlamaIndex 进行查询。
     {% endhint %}
   - **Embeddings** 可以与 [**Embeddings**](../embeddings/) 类别下的任何节点连接

<figure><img src="../../../.gitbook/assets/pinecone_llama_chatflow.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/pinecone_llama_upsert.png" alt=""><figcaption></figcaption></figure>

5. 在 [Pinecone 仪表板](https://app.pinecone.io) 上验证数据是否已成功 upsert：

<figure><img src="../../../.gitbook/assets/pinecone_8.png" alt=""><figcaption></figcaption></figure>