描述：学习如何为您的 Flowise 实例设置聊天流程级别的访问控制

# 聊天流程级别

***

创建聊天流程/代理流程后，默认情况下，您的流程对公众开放。任何拥有聊天流程 ID 的人都可以通过嵌入或 API 运行预测。

如果您想允许特定人员访问和交互，可以通过为该特定聊天流程分配 API 密钥来实现。

## API 密钥

在仪表板中，导航到“API 密钥”部分，您应该可以看到已创建的 DefaultKey。您还可以添加或删除任何密钥。

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 聊天流程

导航到聊天流程，现在您可以选择要用于保护聊天流程的 API 密钥。

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

分配 API 密钥后，只有在 HTTP 调用期间提供包含正确 API 密钥的 Authorization 标头时，才能访问聊天流程 API。

代码块 0 json
```json
"Authorization": "Bearer <您的 API 密钥>"
```

使用 POSTMAN 调用 API 的示例

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

您可以通过指定 `APIKEY_PATH` 环境变量来指定 API 密钥的存储位置。阅读更多信息 [environment-variables.md](../environment-variables.md "mention")
