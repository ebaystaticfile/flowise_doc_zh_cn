（保持原有提示词不变）---
description: 学习如何在 Flowise 中使用变量
---

# 变量

***

Flowise 允许用户创建可在节点中使用的变量。变量可分为静态变量和运行时变量。

### 静态变量

静态变量将保存指定的值，并按原样检索。

<figure><img src="../.gitbook/assets/image (13) (1) (1) (1).png" alt="" width="542"><figcaption></figcaption></figure>

### 运行时变量

变量的值将通过 `process.env` 从 **.env** 文件中获取。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="537"><figcaption></figcaption></figure>

### 通过 API 覆盖或设置变量

要覆盖变量值，用户需在 **Chatflow 配置** -> **安全** 选项卡中明确启用此功能：

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

若已存在同名变量，API 提供的变量值将覆盖现有值。

```json
{
    "question": "hello",
    "overrideConfig": {
        "vars": {
            "var": "some-override-value"
        }
    }
}
```

### 使用变量

变量可在 Flowise 的节点中使用。例如，创建一个名为 **`character`** 的变量：

<figure><img src="../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

随后可通过 **`$vars.<变量名>`** 在以下节点的函数中使用该变量：

* [自定义工具](../integrations/langchain/tools/custom-tool_zh.md)
* [自定义函数](../integrations/utilities/custom-js-function_zh.md)
* [自定义加载器](../integrations/langchain/document-loaders/custom-document-loader_zh.md)
* [条件分支](../integrations/utilities/if-else_zh.md)

<figure><img src="../.gitbook/assets/image (105).png" alt="" width="283"><figcaption></figcaption></figure>

此外，用户也可通过以下格式在任意节点的文本输入中使用变量：

**`{{$vars.<变量名>}}`**

例如，在 Agent 系统消息中：

<figure><img src="../.gitbook/assets/image (1) (1) (1) (2) (1).png" alt="" width="508"><figcaption></figcaption></figure>

在提示模板中：

<figure><img src="../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

## 相关资源

* [向函数传递变量](../integrations/langchain/tools/custom-tool.md#pass-variables-to-function)