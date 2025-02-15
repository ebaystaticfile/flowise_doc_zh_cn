# 本地AI嵌入

## 本地AI设置

[**LocalAI**](https://github.com/go-skynet/LocalAI) 是一个可直接替换的 REST API，兼容 OpenAI API 规范，用于本地推理。它允许您在消费级硬件上本地或内部部署运行大型语言模型 (LLM)（不仅限于 LLM），支持多个与 ggml 格式兼容的模型系列。

要在 Flowise 中使用 LocalAI 嵌入，请按照以下步骤操作：

1. 代码块 0 (bash)
   ```bash
   git clone https://github.com/go-skynet/LocalAI
   ```
   代码块 1
2. <pre class="language-bash"><code class="lang-bash"><strong>cd LocalAI
   </strong></code></pre>
3. LocalAI 提供了一个 [API 端点](https://localai.io/api-endpoints/index.html#applying-a-model---modelsapply) 用于下载/安装模型。在本例中，我们将使用 BERT 嵌入模型：

<figure><img src="../../../.gitbook/assets/image (27) (1).png" alt=""><figcaption></figcaption></figure>

4. 在 `/models` 文件夹中，您应该能够看到已下载的模型：

<figure><img src="../../../.gitbook/assets/image (23) (1).png" alt=""><figcaption></figcaption></figure>

5. 现在您可以测试嵌入：

```bash
curl http://localhost:8080/v1/embeddings -H "Content-Type: application/json" -d '{
    "input": "Test",
    "model": "text-embedding-ada-002"
  }'
```

6. 响应应如下所示：

<figure><img src="../../../.gitbook/assets/image (29).png" alt="" width="375"><figcaption></figcaption></figure>

## Flowise 设置

将新的 LocalAIEmbeddings 组件拖放到画布上：

<figure><img src="../../../.gitbook/assets/image (21) (1) (2).png" alt=""><figcaption></figcaption></figure>

填写以下字段：

* **基本路径**: LocalAI 的基本 URL，例如 [http://localhost:8080/v1](http://localhost:8080/v1)
* **模型名称**: 您要使用的模型。请注意，它必须位于 LocalAI 目录的 `/models` 文件夹内。例如：`text-embedding-ada-002`

就是这样！更多信息，请参考 LocalAI [文档](https://localai.io/models/index.html#embeddings-bert)。
