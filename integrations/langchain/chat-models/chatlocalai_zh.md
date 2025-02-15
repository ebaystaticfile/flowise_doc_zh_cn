# ChatLocalAI

## LocalAI 设置

[**LocalAI**](https://github.com/go-skynet/LocalAI) 是一个可直接替换的 REST API，兼容 OpenAI API 规范，用于本地推理。它允许您在消费级硬件上本地或内部部署运行大型语言模型 (LLM)（以及其他模型），支持多种与 ggml 格式兼容的模型系列。

要在 Flowise 中使用 ChatLocalAI，请按照以下步骤操作：

1. 代码块 0 (bash)
   ```bash
   git clone https://github.com/go-skynet/LocalAI
   ```
   代码块 1
2. `<pre class="language-bash"><code class="lang-bash"><strong>cd LocalAI
   </strong></code></pre>`
3. 代码块 2 (bash)
   ```bash
   # 将您的模型复制到 models/ 目录下
   cp your-model.bin models/
   ```
   代码块 3

例如：

从 [gpt4all.io](https://gpt4all.io/index.html) 下载一个模型

代码块 4 (bash)
```bash
# 将 gpt4all-j 下载到 models/ 目录下
wget https://gpt4all.io/models/ggml-gpt4all-j.bin -O models/ggml-gpt4all-j
```

在 `/models` 文件夹中，您应该可以看到已下载的模型：

<figure><img src="../../../.gitbook/assets/image (22) (1).png" alt=""><figcaption></figcaption></figure>

支持的模型列表，请参考 [此处](https://localai.io/model-compatibility/index.html)。

4. ```bash
   docker compose up -d --pull always
   ```
5. API 现在可在 localhost:8080 访问

```bash
# 测试 API
curl http://localhost:8080/v1/models
# {"object":"list","data":[{"id":"ggml-gpt4all-j.bin","object":"model"}]}
```

## Flowise 设置

将新的 ChatLocalAI 组件拖放到画布上：

<figure><img src="../../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

填写以下字段：

* **基础路径 (Base Path)**：LocalAI 的基础 URL，例如 [http://localhost:8080/v1](http://localhost:8080/v1)
* **模型名称 (Model Name)**：您要使用的模型。请注意，它必须位于 LocalAI 目录的 `/models` 文件夹内。例如：`ggml-gpt4all-j.bin`

{% hint style="info" %}
如果您同时在 Docker 上运行 Flowise 和 LocalAI，您可能需要将基础路径更改为 [http://host.docker.internal:8080/v1](http://host.docker.internal:8080/v1)。对于基于 Linux 的系统，应使用默认的 Docker 网关，因为 host.docker.internal 不可用：[http://172.17.0.1:8080/v1](http://172.17.0.1:8080/v1)
{% endhint %}

就是这样！更多信息，请参考 LocalAI [文档](https://localai.io/basics/getting_started/index.html)。

观看如何在 Flowise 上使用 LocalAI 的视频：

{% embed url="https://youtu.be/0B0oIs8NS9k" %}
