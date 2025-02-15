---
description: Learn how to effectively use the Chatflow Tool and the Custom Tool
---

# 调用子流程

***

Flowise 的强大功能之一是您可以将流程转换为工具。例如，可以使用主流程来协调何时使用必要的工具。每个工具都设计用于执行特定任务。

这带来了一些好处：

* 每个子流程作为工具将独立执行，并拥有独立的内存，从而获得更清晰的输出。
* 将每个子流程的详细输出汇总到最终代理，通常会产生更高质量的输出。

您可以使用以下工具实现此目的：

* Chatflow 工具
* 自定义工具


## Chatflow 工具

1. 准备一个 Chatflow。在本例中，我们创建了一个可以进行多次链式推理的链式思维 Chatflow。

<figure><img src="../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

2. 创建另一个包含工具代理 + Chatflow 工具的 Chatflow。从工具中选择要调用的 Chatflow。在本例中，它是链式思维 Chatflow。为其命名，并提供适当的描述，以便 LLM 知道何时使用此工具：

<figure><img src="../.gitbook/assets/image (35).png" alt="" width="245"><figcaption></figcaption></figure>

3. 测试它！

<figure><img src="../.gitbook/assets/image (168).png" alt=""><figcaption></figcaption></figure>

4. 从响应中，您可以看到 Chatflow 工具的输入和输出：

<figure><img src="../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>


## 自定义工具

使用与上述相同的示例，我们将创建一个自定义工具，该工具将调用链式思维 Chatflow 的 [预测 API](../using-flowise/api.md#prediction-api)。

1. 创建一个新工具：

<table><thead><tr><th width="180">工具名称</th><th>工具描述</th></tr></thead><tbody><tr><td>ideas_flow</td><td>当您需要实现特定目标时，使用此工具</td></tr></tbody></table>

输入模式：

<table><thead><tr><th>属性</th><th>类型</th><th>描述</th><th data-type="checkbox">必填</th></tr></thead><tbody><tr><td>input</td><td>string</td><td>输入问题</td><td>true</td></tr></tbody></table>

<figure><img src="../.gitbook/assets/image (95) (1).png" alt=""><figcaption></figcaption></figure>

工具的 JavaScript 函数：

```javascript
const fetch = require('node-fetch');
const url = 'http://localhost:3000/api/v1/prediction/<chatflow-id>'; // 请替换为具体的 chatflow ID

const body = {
	"question": $input
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

try {
	const response = await fetch(url, options);
	const resp = await response.json();
	return resp.text;
} catch (error) {
	console.error(error);
	return '';
}
```

2. 创建一个工具代理 + 自定义工具。在自定义工具中指定我们在步骤 1 中创建的工具。

<figure><img src="../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

3. 从响应中，您可以看到自定义工具的输入和输出：

<figure><img src="../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>


## 结论

在本例中，我们成功地演示了两种将其他 Chatflow 转换为工具的方法，分别通过 Chatflow 工具和自定义工具。两者在底层使用相同的代码逻辑。
