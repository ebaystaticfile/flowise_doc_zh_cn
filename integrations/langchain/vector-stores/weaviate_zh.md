描述：>
使用Weaviate（一个可扩展的开源向量数据库）上传嵌入式数据并执行相似性或MMR搜索。
---

# Weaviate

<figure><img src="../../../.gitbook/assets/image (165).png" alt="" width="295"><figcaption>Weaviate 节点</figcaption></figure>

## 过滤

Weaviate 在过滤方面支持以下[语法](https://weaviate.io/developers/weaviate/search/filters)：

**用户界面**

<figure><img src="../../../.gitbook/assets/image (5) (1) (1).png" alt="" width="227"><figcaption></figcaption></figure>

**API**

```json
```json
"overrideConfig": {
    "weaviateFilter": {
        "where": {
            "operator": "Equal",
            "path": [
                "test"
            ],
            "valueText": "key"
        }
    }
}
```

## 资源

* [LangchainJS Weaviate](https://js.langchain.com/v0.1/docs/integrations/vectorstores/weaviate/#usage-query-documents)
* [Weaviate 过滤](https://weaviate.io/developers/weaviate/search/filters)

{% hint style="info" %}
本节内容正在建设中。感谢您提供任何帮助以完成本节内容。请查看我们的[贡献指南](../../../contributing/)以开始。
{% endhint %}
