---
description: 学习如何在 Flowise 中使用外部 API 集成
---

# 与 API 交互

***

OpenAPI 规范 (OAS) 定义了与 HTTP API 交互的标准、与语言无关的接口。此用例的目标是让 LLM 自动确定要调用的 API，同时仍能与用户进行有状态的对话。

## OpenAPI 链

1. 在本教程中，我们将使用 [Klarna OpenAPI](https://gist.github.com/HenryHengZJ/b60f416c42cb9bcd3160fe797421119a)

{% code overflow="wrap" %}
```json
{
  "openapi": "3.0.1",
  "info": {
    "version": "v0",
    "title": "Open AI Klarna product Api"
  },
  "servers": [
    {
      "url": "https://www.klarna.com/us/shopping"
    }
  ],
  "tags": [
    {
      "name": "open-ai-product-endpoint",
      "description": "Open AI 产品端点。查询产品。"
    }
  ],
  "paths": {
    "/public/openai/v0/products": {
      "get": {
        "tags": [
          "open-ai-product-endpoint"
        ],
        "summary": "用于获取 Klarna 产品信息的 API",
        "operationId": "productsUsingGET",
        "parameters": [
          {
            "name": "countryCode",
            "in": "query",
            "description": "基于用户位置的两位 ISO 3166 国家代码。目前仅支持美国、英国、德国、瑞典和丹麦。",
            "required": true,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "q",
            "in": "query",
            "description": "精确查询，匹配需要搜索以查找用户正在寻找的产品的一个非常小的类别或产品。如果用户明确说明了他们的需求，请将其用作查询。查询尽可能具体到产品名称或用户在其单数形式中提到的类别，并且不包含最新的、最新的、最便宜的、预算的、高级的、昂贵的或类似的任何说明符。查询始终取自最新的主题，如果有新主题，则会启动新的查询。如果用户使用英语以外的语言，请将其请求翻译成英语（例如：将 fia med knuff 翻译成 ludo board game）！",
            "required": true,
            "schema": {
              "type": "string"
            }
          },
          {
            "name": "size",
            "in": "query",
            "description": "返回的产品数量",
            "required": false,
            "schema": {
              "type": "integer"
            }
          },
          {
            "name": "min_price",
            "in": "query",
            "description": "(可选) 搜索产品的最低价格（以当地货币计）。由用户明确说明或从用户的请求和搜索的产品类型组合中隐含推断。",
            "required": false,
            "schema": {
              "type": "integer"
            }
          },
          {
            "name": "max_price",
            "in": "query",
            "description": "(可选) 搜索产品的最高价格（以当地货币计）。由用户明确说明或从用户的请求和搜索的产品类型组合中隐含推断。",
            "required": false,
            "schema": {
              "type": "integer"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "找到产品",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/ProductResponse"
                }
              }
            }
          },
          "503": {
            "description": "一个或多个服务不可用"
          }
        },
        "deprecated": false
      }
    }
  },
  "components": {
    "schemas": {
      "Product": {
        "type": "object",
        "properties": {
          "attributes": {
            "type": "array",
            "items": {
              "type": "string"
            }
          },
          "name": {
            "type": "string"
          },
          "price": {
            "type": "string"
          },
          "url": {
            "type": "string"
          }
        },
        "title": "Product"
      },
      "ProductResponse": {
        "type": "object",
        "properties": {
          "products": {
            "type": "array",
            "items": {
              "$ref": "#/components/schemas/Product"
            }
          }
        },
        "title": "ProductResponse"
      }
    }
  }
}
```
{% endcode %}

2. 你可以使用 [JSON 到 YAML 转换器](https://jsonformatter.org/json-to-yaml) 并将其保存为 `.yaml` 文件，然后将其上传到 **OpenAPI 链**，然后通过提出一些问题进行测试。**OpenAPI 链** 将将整个规范发送到 LLM，并让 LLM 自动使用 API 调用的正确方法和参数。

<figure><img src="../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

3. 但是，如果你想进行正常的对话聊天，则无法做到这一点。你将看到以下错误。这是因为 OpenAPI 链具有以下提示：

```
使用提供的 API 来响应此用户查询
```

由于我们“强制”它始终查找 API 来回答用户查询，因此在与 OpenAPI 无关的正常对话情况下，它会失败。

<figure><img src="../.gitbook/assets/image (134).png" alt="" width="361"><figcaption></figcaption></figure>

如果你的 OpenAPI 规范很大，则此方法可能无法正常工作。这是因为我们将所有规范都作为发送到 LLM 的消息的一部分。然后，我们依靠 LLM 来确定回答用户查询所需的正确 URL、查询参数、请求正文和其他必要的参数。可以想象，如果你的 OpenAPI 规范很复杂，LLM 产生幻觉的可能性就越高。

## 工具代理 + OpenAPI 工具包

为了解决上述错误，我们可以使用代理。根据 OpenAI 的官方食谱：[使用 OpenAPI 规范进行函数调用](https://cookbook.openai.com/examples/function_calling_with_an_openapi_spec)，建议将每个 API 转换为一个工具本身，而不是将所有 API 作为单个消息馈送到 LLM。代理还能够进行类似于人类的交互，并能够根据用户的查询决定使用哪个工具。

OpenAPI 工具包会将 YAML 文件中的每个 API 转换为一组工具。这样，用户不必为每个 API 创建 [自定义工具](../integrations/langchain/tools/custom-tool_zh.md)。

1. 将 **工具代理** 与 **OpenAPI 工具包** 连接。在这里，我们上传 OpenAI API 的 YAML 规范。规范文件可以在页面底部找到。

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

2. 让我们试试！

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

正如你从聊天中注意到的那样，代理能够在必要时与 API 交互，并且仍然能够处理与用户的有状态对话。如果你使用的是分析工具，你可以看到我们从 YAML 文件转换的工具列表：

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 结论

我们已经成功创建了一个代理，它可以在必要时与 API 交互，并且仍然能够处理与用户的有状态对话。以下是本节中使用的模板：

{% file src="../.gitbook/assets/OpenAPI Chatflow.json" %}

{% file src="../.gitbook/assets/OpenAPI Toolkit with ToolAgent Chatflow.json" %}

{% file src="../.gitbook/assets/openai_openapi.yaml" %}
