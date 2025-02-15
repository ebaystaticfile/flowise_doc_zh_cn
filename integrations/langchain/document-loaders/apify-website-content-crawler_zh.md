---
description: 从 Apify 网站内容爬虫加载数据。
---

# Apify 网站内容爬虫

[Apify](https://apify.com/) 是一个网页抓取和数据提取平台，提供了一个应用商店，其中包含超过一千个名为 Actor 的现成云工具。

[网站内容爬虫](https://apify.com/apify/website-content-crawler) Actor 可以深度爬取网站，通过移除 Cookie 模态框、页脚或导航来清理其 HTML，然后将 HTML 转换为 Markdown。此 Markdown 随后可以存储在向量数据库中，用于语义搜索或检索增强生成 (RAG)。

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="266"><figcaption><p>Apify 网站内容爬虫节点</p></figcaption></figure>

## 爬取整个网站

1. _(可选)_ 连接 [**文本分割器**](../text-splitters/)。
2. 连接 Apify API（使用您的 [Apify API 令牌](https://my.apify.com/account#/integrations) 创建新的凭据）。
3. 输入一个或多个 URL（用逗号分隔），爬虫将从此处开始，例如 `https://docs.flowiseai.com/`。
4. 选择爬虫类型。请参考 [网站内容爬虫文档了解更多信息](https://apify.com/apify/website-content-crawler/input-schema#crawlerType)。
5. _(可选)_ 指定其他参数，例如最大爬取深度和最大爬取页面数。

## 输出

将网站内容加载为文档。

## 资源

* [Apify-Flowise 集成](https://docs.apify.com/platform/integrations/flowise)
* [网站内容爬虫](https://apify.com/apify/website-content-crawler)
