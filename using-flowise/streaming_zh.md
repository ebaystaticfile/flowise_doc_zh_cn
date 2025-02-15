---
description: Learn how Flowise streaming works
---

# 流式处理

如果在进行预测时设置了流式处理，则令牌将作为仅包含数据的[服务器发送事件](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events#Event_stream_format)在可用时发送。

### 使用 Python/TypeScript 库

Flowise 提供两个库：

* [Python](https://pypi.org/project/flowise/): `pip install flowise`
* [TypeScript](https://www.npmjs.com/package/flowise-sdk): `npm install flowise-sdk`

{% tabs %}
{% tab title="Python" %}
```python
from flowise import Flowise, PredictionData

def test_streaming():
    client = Flowise()

    # 测试流式预测
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<chatflow-id>",
            question="Tell me a joke!",
            streaming=True
        )
    )

    # 处理并打印每个流式片段
    print("流式响应：")
    for chunk in completion:
        # {event: "token", data: "hello"}
        print(chunk)


if __name__ == "__main__":
    test_streaming()
```
{% endtab %}

{% tab title="TypeScript" %}
```javascript
import { FlowiseClient } from 'flowise-sdk'

async function test_streaming() {
  const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });

  try {
    // 用于流式预测
    const prediction = await client.createPrediction({
      chatflowId: '<chatflow-id>',
      question: 'What is the capital of France?',
      streaming: true,
    });

    for await (const chunk of prediction) {
        // {event: "token", data: "hello"}
        console.log(chunk);
    }
    
  } catch (error) {
    console.error('错误：', error);
  }
}

// 运行流式测试
test_streaming()
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl https://localhost:3000/api/v1/predictions/{chatflow-id} \
  -H "Content-Type: application/json" \
  -d '{
    "question": "Hello world!",
    "streaming": true
  }'
```
{% endtab %}
{% endtabs %}

```html
event: token
data: 从前从前……
```

预测的事件流包含以下事件类型：

| 事件           | 描述                                                                                                                         |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| start           | 流式处理开始                                                                                                              |
| token           | 当预测正在流式传输新的令牌输出时发出                                                                           |
| error           | 当预测返回错误时发出                                                                                                                       |
| end             | 当预测完成时发出                                                                                                |
| metadata        | 所有元数据，例如相关流程的 chatId、messageId。在所有令牌流式传输完成之后以及 end 事件之前发出 |
| sourceDocuments | 当流程从向量存储返回来源时发出                                                                             |
| usedTools       | 当流程使用工具时发出                                                                                                    |

### Streamlit 应用

[https://github.com/HenryHengZJ/flowise-streamlit](https://github.com/HenryHengZJ/flowise-streamlit)
