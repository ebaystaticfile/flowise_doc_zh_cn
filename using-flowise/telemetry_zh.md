---
description: 了解 Flowise 如何收集匿名的应用程序使用信息
---

# 遥测数据

***

Flowise 开源存储库内置了遥测功能，用于收集匿名的使用信息。这有助于我们更好地了解 Flowise 的使用情况，从而能够优先处理开发新功能和解决问题的工作，并提高 Flowise 的性能和稳定性。

{% hint style="warning" %}
<mark style="color:red;">**重要提示**</mark> - 我们绝不会收集任何关于节点输入/输出、消息或任何类型的凭据和变量的机密信息。仅发送事件。
{% endhint %}

您可以通过查找源代码中所有调用 `telemetry.sendTelemetry` 的位置来验证这些声明。

<table><thead><tr><th width="238">事件</th><th>元数据</th></tr></thead><tbody><tr><td>chatflow_created</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;应用程序版本>,
    "chatlowId": &#x3C;聊天流程 ID>,
    "flowGraph": {
        "nodes": [&#x3C;节点 ID 1>, &#x3C;节点 ID 2>],
        "edges": [
            {
                "source": &#x3C;节点 ID 1>,
                "target": &#x3C;节点 ID 2>
            }
        ]
    }
}
</code></pre></td></tr><tr><td>tool_created</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;应用程序版本>,
    "toolId": &#x3C;工具 ID>,
    "toolName": &#x3C;工具名称>
}
</code></pre></td></tr><tr><td>assistant_created</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;应用程序版本>,
    "assistantId": &#x3C;助手 ID>
}
</code></pre></td></tr><tr><td>vector_upserted</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;应用程序版本>,
    "chatlowId": &#x3C;聊天流程 ID>,
    "type": "INTERNAL", // EXTERNAL
    "flowGraph": {
        "nodes": [&#x3C;节点 ID 1>, &#x3C;节点 ID 2>],
        "edges": [
            {
                "source": &#x3C;节点 ID 1>,
                "target": &#x3C;节点 ID 2>
            }
        ]
    },
    "stopNodeId": &#x3C;节点 ID 1>
}
</code></pre></td></tr><tr><td>prediction_sent</td><td><pre class="language-json"><code class="lang-json">{
    "version": &#x3C;应用程序版本>,
    "chatlowId": &#x3C;聊天流程 ID>,
    "chatId": &#x3C;聊天 ID>,
    "type": "INTERNAL", // EXTERNAL
    "flowGraph": {
        "nodes": [&#x3C;节点 ID 1>, &#x3C;节点 ID 2>],
        "edges": [
            {
                "source": &#x3C;节点 ID 1>,
                "target": &#x3C;节点 ID 2>
            }
        ]
    }
}
</code></pre></td></tr></tbody></table>

## 禁用遥测数据

用户可以通过在 `.env` 文件中将 `DISABLE_FLOWISE_TELEMETRY` 设置为 `true` 来禁用遥测数据。
