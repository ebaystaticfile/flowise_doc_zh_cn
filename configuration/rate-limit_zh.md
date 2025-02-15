---
description: 如何在 Flowise 中管理 API 请求
---

# 速率限制

***

当您通过 API 或嵌入式聊天将聊天流程公开共享且未进行 API 授权时，任何人都可以访问该流程。为防止垃圾信息，您可以为聊天流程设置速率限制。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="462"><figcaption></figcaption></figure>

* **每段时间的消息限制**: 在特定时间段内可以接收的消息数量。例如：20
* **持续时间（秒）**: 指定的持续时间。例如：60
* **限制消息**: 超出限制时返回的消息。例如：配额已用尽

以上示例表示，在 60 秒内仅允许接收 20 条消息。速率限制通过 IP 地址进行跟踪。如果您已在云服务上部署 Flowise，则必须设置 `NUMBER_OF_PROXIES` 环境变量。


## 速率限制设置

当您在 AWS、GCP、Azure 等云平台上托管 Flowise 时，您很可能位于代理/负载均衡器之后。因此，速率限制可能无法正常工作。更多信息请参见 [此处](https://github.com/express-rate-limit/express-rate-limit/wiki/Troubleshooting-Proxy-Issues)。

要解决此问题：

1. **设置环境变量**: 在您的托管环境中创建一个名为 `NUMBER_OF_PROXIES` 的环境变量，并将其值设置为 `0`。
2. **重启您的 Flowise 实例**: 这将使 Flowise 应用环境变量的更改。
3. **检查 IP 地址**: 要验证 IP 地址，请访问以下 URL：`{{hosted_url}}/api/v1/ip`。您可以通过在 Web 浏览器中输入 URL 或发出 API 请求来执行此操作。
4. **比较 IP 地址**: 发出请求后，将返回的 IP 地址与您当前的 IP 地址进行比较。您可以访问以下任一网站查找您的当前 IP 地址：
   * [http://ip.nfriedly.com/](http://ip.nfriedly.com/)
   * [https://api.ipify.org/](https://api.ipify.org/)
5. **IP 地址不正确**: 如果返回的 IP 地址与您当前的 IP 地址不匹配，请将 `NUMBER_OF_PROXIES` 增加 1 并重启您的 Flowise 实例。重复此过程，直到 IP 地址与您自己的 IP 地址匹配。
