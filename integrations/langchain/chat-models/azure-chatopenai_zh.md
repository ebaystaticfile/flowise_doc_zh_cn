# ChatOpenAI

## 先决条件

1. 一个 [OpenAI](https://openai.com/) 账户
2. 创建一个 [API 密钥](https://platform.openai.com/api-keys)

## 设置

1. **聊天模型** > 拖动 **ChatOpenAI** 节点

<figure><img src="../../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

2. **连接凭据** > 点击 **创建新的**

<figure><img src="../../../.gitbook/assets/image_openAI (1).png" alt="" width="278"><figcaption></figcaption></figure>

3. 填写 **ChatOpenAI** 凭据

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

4. 完成！🎉  您现在可以在 Flowise 中使用 **ChatOpenAI 节点**

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 自定义基础 URL 和标头

Flowise 支持为 Chat OpenAI 使用自定义基础 URL 和标头。用户可以轻松使用与 OpenAI API 兼容的集成，例如 OpenRouter、TogetherAI 等。

### TogetherAI

1. 参考 TogetherAI 的官方 [文档](https://docs.together.ai/docs/openai-api-compatibility#nodejs)
2. 使用 TogetherAI API 密钥创建一个新的凭据
3. 点击 ChatOpenAI 节点上的 **附加参数**。
4. 更改基础路径：

<figure><img src="../../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

### Open Router

1. 参考 OpenRouter 的官方 [文档](https://openrouter.ai/docs#quick-start)
2. 使用 OpenRouter API 密钥创建一个新的凭据
3. 点击 ChatOpenAI 节点上的附加参数
4. 更改基础路径和基础选项：

<figure><img src="../../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

## 自定义模型

对于 ChatOpenAI 节点不支持的模型，您可以使用 ChatOpenAI 自定义功能。这允许用户填写模型名称，例如 `mistralai/Mixtral-8x7B-Instruct-v0.1`

<figure><img src="../../../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>

## 图片上传

您还可以允许上传和分析图像。Flowise 将在后台使用 [OpenAI 视觉](https://platform.openai.com/docs/guides/vision) 模型来处理图像。仅适用于 LLMChain、对话链、ReAct 代理和对话代理。

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="332"><figcaption></figcaption></figure>

在聊天界面中，您现在将看到一个新的图片上传按钮：

<figure><img src="../../../.gitbook/assets/Untitled (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>
