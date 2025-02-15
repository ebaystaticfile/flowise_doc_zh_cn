# 生产环境运行

## 模式

在生产环境中运行时，我们强烈建议使用带有以下设置的[队列](running-flowise-using-queue_zh.md)模式：

* 至少两台主服务器，使用负载均衡，每台服务器的起始配置为 2 个 CPU 和 4GB RAM
* 至少两个工作节点，每台工作节点的起始配置为 1 个 CPU 和 2GB RAM

您可以根据流量和数据量配置自动伸缩。

## 数据库

Flowise 默认使用 SQLite 作为数据库。但是，在大规模运行时，建议使用 PostgreSQL。

## 存储

目前 Flowise 只支持[AWS S3](https://aws.amazon.com/s3/)，并计划支持更多 Blob 存储提供商。这允许将文件和日志存储在 S3 上，而不是本地文件路径。请参考 [#for-storage](environment-variables.md#for-storage "mention")

## 加密

Flowise 使用加密密钥来加密/解密您使用的凭据，例如 OpenAI API 密钥。建议在生产环境中使用[AWS Secret Manager](https://aws.amazon.com/secrets-manager/)，以获得更好的安全控制和密钥轮换。请参考 [#for-credentials](environment-variables.md#for-credentials "mention")

## API 密钥存储

用户可以在 Flowise 中创建多个 API 密钥以对[API](../using-flowise/api_zh.md)进行身份验证。默认情况下，密钥将作为 JSON 文件存储到您的本地文件路径。但是，当您有多个实例时，每个实例都会创建一个新的 JSON 文件，从而导致混淆。您可以更改行为，改为将密钥存储到数据库中。请参考 [#for-flowise-api-keys](environment-variables.md#for-flowise-api-keys "mention")

## 速率限制

当部署到云端/本地环境时，实例很可能位于代理/负载均衡器之后。请求的 IP 地址可能是负载均衡器/反向代理的 IP 地址，这使得速率限制器实际上成为全局速率限制器，一旦达到限制或 `undefined`，就会阻止所有请求。设置正确的 `NUMBER_OF_PROXIES` 可以解决此问题。请参考 [#rate-limit-setup](rate-limit.md#rate-limit-setup "mention")

## 负载测试

可以使用 Artillery 对已部署的 Flowise 应用程序进行负载测试。示例脚本可以在这里找到：[here](https://github.com/FlowiseAI/Flowise/blob/main/artillery-load-test.yml)。
