# 在公司代理服务器后运行 Flowise

如果您在需要代理的环境（例如组织网络）中运行 Flowise，您可以配置 Flowise 将其所有后端请求通过您选择的代理路由。此功能由 `global-agent` 包提供支持。

[https://github.com/gajus/global-agent](https://github.com/gajus/global-agent)

## 配置

在公司代理服务器后运行 Flowise 需要设置 2 个环境变量：

| 变量                     | 目的                                                                              | 是否必需 |
| ------------------------ | ---------------------------------------------------------------------------------- | -------- |
| `GLOBAL_AGENT_HTTP_PROXY` | 所有服务器 HTTP 请求的代理地址                                                   | 是       |
| `GLOBAL_AGENT_HTTPS_PROXY`| 所有服务器 HTTPS 请求的代理地址                                                  | 否       |
| `GLOBAL_AGENT_NO_PROXY`   | 应从代理中排除的 URL 模式。例如：`*.foo.com,baz.com`                             | 否       |

## 出站允许列表

对于企业版，您必须允许几个出站连接以进行许可证检查。更多信息，请联系 support@flowiseai.com。
