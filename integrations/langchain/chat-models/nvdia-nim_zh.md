# NVIDIA NIM

## 先决条件

1. 登录或注册 NVIDIA 账号 ([Nvdia](https://build.nvidia.com/))。
2. 在顶部导航栏中，点击 NIM：

<figure><img src="../../../.gitbook/assets/image (247).png" alt=""><figcaption></figcaption></figure>

3. 搜索您想要使用的模型。要将其下载到本地，我们将使用 Docker：

<figure><img src="../../../.gitbook/assets/image (248).png" alt=""><figcaption></figcaption></figure>

4. 按照 Docker 设置说明进行操作。您必须首先获取 API 密钥才能拉取 Docker 镜像：

<figure><img src="../../../.gitbook/assets/image (249).png" alt="" width="563"><figcaption></figcaption></figure>

## Flowise

1. **聊天模型** > 拖动 **Chat NVIDIA NIM** 节点

<figure><img src="../../../.gitbook/assets/image (250).png" alt=""><figcaption></figcaption></figure>

2. 如果您使用的是 NVIDIA 托管端点，则必须拥有您的 API 密钥。**连接凭据** > 点击 **创建新的**。但是，如果您使用的是本地设置，则此步骤可选。

<div align="left"><figure><img src="../../../.gitbook/assets/image (251).png" alt=""><figcaption></figcaption></figure> <figure><img src="../../../.gitbook/assets/Screenshot 2024-12-23 180712.png" alt=""><figcaption></figcaption></figure></div>

3. 输入模型名称，然后大功告成 [🎉](https://emojipedia.org/party-popper/)，您的 **NVIDIA NIM 节点** 现在就可以在 Flowise 中使用了！

<figure><img src="../../../.gitbook/assets/image (252).png" alt=""><figcaption></figcaption></figure>

## 资源

* [NVIDIA LLM 入门](https://docs.nvidia.com/nim/large-language-models/latest/getting-started.html)
* [NVIDIA NIM](https://build.nvidia.com/microsoft/phi-3-mini-4k?snippet_tab=Docker)
