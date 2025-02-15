---
description: 自定义检索器(Custom Retriever)允许用户指定上下文格式供LLM使用
---

# 自定义检索器(Custom Retriever)

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt="" width="298"><figcaption></figcaption></figure>

默认情况下，当从向量存储(vector store)中检索上下文时，它们的格式如下：

```json
[ 
    {
        "pageContent": "This is an example",
        "metadata": {
            "source": "example.pdf"
        }
    },
    {
        "pageContent": "This is example 2",
        "metadata": {
            "source": "example2.txt"
        }
    }
]
```

数组的**pageContent**将被连接成一个字符串，并反馈给LLM进行补全。

然而，在某些情况下，您可能希望包含元数据(metadata)中的信息，以向LLM提供更多信息，例如来源、链接等。这就是**自定义检索器(Custom Retriever)**的用武之地。我们可以指定返回给LLM的格式。

例如，使用以下格式：

```javascript
{{context}}
Source: {{metadata.source}}
```

将生成如下组合字符串：

```
This is an example
Source: example.pdf

This is example 2
Source: example2.txt
```

这将发送回LLM。由于LLM现在有了答案的来源，我们可以使用提示(prompts)来指示LLM返回答案并附上引用。