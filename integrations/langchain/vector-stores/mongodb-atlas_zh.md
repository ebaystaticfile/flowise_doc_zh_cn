使用MongoDB Atlas（托管的云MongoDB数据库）上传嵌入式数据，并根据查询执行相似性或MMR搜索。


# MongoDB Atlas

<figure><img src="../../../.gitbook/assets/image (161).png" alt="" width="308"><figcaption>MongoDB Atlas 节点</figcaption></figure>

### 集群配置[​](https://js.langchain.com/docs/integrations/vectorstores/mongodb_atlas/#initial-cluster-configuration) <a href="#initial-cluster-configuration" id="initial-cluster-configuration"></a>

要设置MongoDB Atlas集群，请访问[MongoDB Atlas](https://www.mongodb.com/)网站，如果您没有帐户，请注册。出现提示时，创建并命名您的集群，它将显示在“数据库”部分下。然后，选择“**浏览集合**”以创建新集合或使用提供的示例数据中的集合。

{% hint style="warning" %}
请确保您创建的集群版本为7.0或更高版本。
{% endhint %}

### 创建索引

设置集群后，下一步是为要搜索的集合字段创建索引。

1. 转到**Atlas Search**选项卡，然后单击**创建搜索索引**。
2. 选择**Atlas Vector Search - JSON 编辑器**，选择相应的数据库和集合，然后将以下内容粘贴到文本框中：

```json
```json
{
  "fields": [
    {
      "numDimensions": 1536,
      "path": "embedding",
      "similarity": "euclidean",
      "type": "vector"
    }
  ]
}
```

确保`numDimensions`属性与您使用的嵌入的维度相对应。例如，Cohere嵌入通常具有1024个维度，而OpenAI嵌入默认具有1536个维度。

**注意：**向量存储需要某些默认值，例如：

* 索引名称为`default`
* 集合字段名称为`embedding`
* 原始文本字段名称为`text`

请确保使用与您的索引和集合模式匹配的字段名称初始化向量存储，如上例所示。

完成后，继续构建索引。

{% hint style="info" %}
本节正在建设中。感谢您提供任何帮助来完成本节。请查看我们的[贡献指南](../../../contributing/)以开始。
{% endhint %}

### Flowise 配置

拖放MongoDB Atlas向量存储，并添加新的凭据。使用MongoDB Atlas仪表板提供的连接字符串：

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

填写其余字段：

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="252"><figcaption></figcaption></figure>

您还可以从“附加参数”中配置更多详细信息：

<figure><img src="../../../.gitbook/assets/image (164).png" alt="" width="518"><figcaption></figcaption></figure>
