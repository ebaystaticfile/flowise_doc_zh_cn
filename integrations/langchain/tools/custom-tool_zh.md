# 自定义工具

观看如何使用自定义工具

{% embed url="https://youtu.be/HSp9LkkTVY0" %}

## 问题

函数通常接收结构化的输入数据。假设您希望大型语言模型 (LLM) 能够调用 Airtable 创建记录 [API](https://airtable.com/developers/web/api/create-records)，则主体参数必须以特定方式进行结构化。例如：

代码块 0json
```json
"records": [
  {
    "fields": {
      "Address": "some address",
      "Name": "some name",
      "Visited": true
    }
  }
]
```
代码块 1

理想情况下，我们希望 LLM 返回如下所示的正确结构化数据：

代码块 2json
```json
{
  "Address": "some address",
  "Name": "some name",
  "Visited": true
}
```
代码块 3

这样我们就可以提取值并将其解析为 API 所需的主体。但是，指示 LLM 输出完全相同的模式非常困难。

借助新的 [OpenAI 函数调用](https://openai.com/blog/function-calling-and-other-api-updates) 模型，现在可以实现此目标。`gpt-4-0613` 和 `gpt-3.5-turbo-0613` 经过专门训练以返回结构化数据。模型将智能地选择输出包含调用这些函数的参数的 JSON 对象。

## 教程

**目标**: 让代理自动获取股票价格变动、检索相关的股票新闻，并向 Airtable 添加新记录。

让我们开始吧 [🚀](https://emojipedia.org/rocket/)

### 创建工具

我们需要三个工具来实现目标：

* 获取股票价格变动
* 获取股票新闻
* 添加 Airtable 记录

#### 获取股票价格变动

创建一个具有以下详细信息的新工具（您可以根据需要更改）：

* 名称：`get_stock_movers`
* 描述：获取价格/成交量变动最大的股票，例如活跃股、上涨股、下跌股等。

描述非常重要，因为 ChatGPT 依靠它来决定何时使用此工具。

<figure><img src="../../../.gitbook/assets/image (6) (3).png" alt=""><figcaption></figcaption></figure>

* JavaScript 函数：我们将使用 [Morning Star](https://rapidapi.com/apidojo/api/morning-star) `/market/v2/get-movers` API 获取数据。如果您尚未订阅，则必须先点击“订阅以测试”，然后复制代码并将其粘贴到 JavaScript 函数中。
  * 在顶部添加 `const fetch = require('node-fetch');` 以导入库。您可以导入任何内置的 NodeJS [模块](https://www.w3schools.com/nodejs/ref_modules.asp) 和 [外部库](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289)。
  * 最后返回 `result`。

<figure><img src="../../../.gitbook/assets/Untitled (4) (1).png" alt=""><figcaption></figcaption></figure>

最终代码应如下所示：

代码块 4javascript
```javascript
const fetch = require('node-fetch');
const url = 'https://morning-star.p.rapidapi.com/market/v2/get-movers';
const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': '替换为您的 API 密钥',
		'X-RapidAPI-Host': 'morning-star.p.rapidapi.com'
	}
};

try {
	const response = await fetch(url, options);
	const result = await response.text();
	console.log(result);
	return result;
} catch (error) {
	console.error(error);
	return '';
}
```
代码块 5

现在您可以保存它了。

#### 获取股票新闻

创建一个具有以下详细信息的新工具（您可以根据需要更改）：

* 名称：`get_stock_news`
* 描述：获取股票的最新新闻
* 输入模式：
  * 属性：`performanceId`
  * 类型：字符串
  * 描述：股票的 ID，在 API 中称为 performanceID
  * 必填：是

输入模式告诉 LLM 返回什么 JSON 对象。在本例中，我们期望如下所示的 JSON 对象：

<pre class="language-json"><code class="lang-json"><strong>{ "performanceId": "SOME TICKER" }
</strong></code></pre>

<figure><img src="../../../.gitbook/assets/image (4) (2).png" alt=""><figcaption></figcaption></figure>

* JavaScript 函数：我们将使用 [Morning Star](https://rapidapi.com/apidojo/api/morning-star) `/news/list` API 获取数据。如果您尚未订阅，则必须先点击“订阅以测试”，然后复制代码并将其粘贴到 JavaScript 函数中。
  * 在顶部添加 `const fetch = require('node-fetch');` 以导入库。您可以导入任何内置的 NodeJS [模块](https://www.w3schools.com/nodejs/ref_modules.asp) 和 [外部库](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289)。
  * 最后返回 `result`。
* 接下来，将硬编码的 url 查询参数 `performanceId: 0P0000OQN8` 替换为输入模式中指定的属性变量：`$performanceId`
* 您可以通过在变量名前添加前缀 `$` 来使用输入模式中指定的任何属性作为 JavaScript 函数中的变量。

<figure><img src="../../../.gitbook/assets/Untitled (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

最终代码：

代码块 6javascript
```javascript
const fetch = require('node-fetch');
const url = 'https://morning-star.p.rapidapi.com/news/list?performanceId=' + $performanceId;
const options = {
	method: 'GET',
	headers: {
		'X-RapidAPI-Key': '替换为您的 API 密钥',
		'X-RapidAPI-Host': 'morning-star.p.rapidapi.com'
	}
};

try {
	const response = await fetch(url, options);
	const result = await response.text();
	console.log(result);
	return result;
} catch (error) {
	console.error(error);
	return '';
}
```
代码块 7

现在您可以保存它了。

#### 添加 Airtable 记录

创建一个具有以下详细信息的新工具（您可以根据需要更改）：

* 名称：`add_airtable`
* 描述：将股票、新闻摘要和价格变动添加到 Airtable
* 输入模式：
  * 属性：`stock`
  * 类型：字符串
  * 描述：股票代码
  * 必填：是
  * 属性：`move`
  * 类型：字符串
  * 描述：价格变动百分比
  * 必填：是
  * 属性：`news_summary`
  * 类型：字符串
  * 描述：股票新闻摘要
  * 必填：是

ChatGPT 将返回如下所示的 JSON 对象：

代码块 8json
```json
{ "stock": "SOME TICKER", "move": "20%", "news_summary": "Some summary" }
```
代码块 9

<figure><img src="../../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

* JavaScript 函数：我们将使用 [Airtable 创建记录 API](https://airtable.com/developers/web/api/create-records) 向现有表中创建一个新记录。您可以从 [此处](https://www.highviewapps.com/kb/where-can-i-find-the-airtable-base-id-and-table-id/) 找到 tableId 和 baseId。您还需要创建一个个人访问令牌，了解如何操作请访问 [此处](https://www.highviewapps.com/kb/how-do-i-create-an-airtable-personal-access-token/)。

最终代码应如下所示。请注意我们如何传入 `$stock`、`$move` 和 `$news_summary` 作为变量：

代码块 10javascript
```javascript
const fetch = require('node-fetch');
const baseId = '您的 base ID';
const tableId = '您的 table ID';
const token = '您的令牌';

const body = {
	"records": [
		{
			"fields": {
				"stock": $stock,
				"move": $move,
				"news_summary": $news_summary,
			}
		}
	]
};

const options = {
	method: 'POST',
	headers: {
		'Authorization': `Bearer ${token}`,
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

const url = `https://api.airtable.com/v0/${baseId}/${tableId}`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```
代码块 11

现在您可以保存它了。

您应该会看到创建了三个工具：

<figure><img src="../../../.gitbook/assets/image (3) (3) (1).png" alt=""><figcaption></figcaption></figure>

### 创建 Chatflow

您可以使用市场中的 **OpenAI 函数** **代理** 模板，并将工具替换为 **自定义工具**。选择您创建的工具。

注意：OpenAI 函数代理目前仅支持 0613 模型。

<figure><img src="../../../.gitbook/assets/image (15) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

保存 chatflow 并开始测试它。对于初学者，您可以尝试询问：

_<mark style="color:blue;">今天价格波动最大的股票是什么？</mark>_

_<mark style="color:orange;">今天价格波动最大的股票是 Overstock.com (OSTK)，价格变动为 17.47%。</mark>_

然后，您可以继续提出另一个问题以获取该特定股票的新闻：

_<mark style="color:blue;">导致价格变动的关于这只股票的最新消息是什么？</mark>_

_<mark style="color:orange;">以下是可能导致 Overstock.com (OSTK) 价格变动的最新消息：</mark>_

1. _<mark style="color:orange;">标题：“Overstock 成功以 2150 万美元竞标 Bed Bath & Beyond 资产，股价飙升” 来源：MarketWatch 发布日期：2023 年 6 月 22 日 摘要：Overstock.com 成功竞标获得价值 2150 万美元的 Bed Bath & Beyond 资产后，其股价大幅上涨。</mark>_
2. _<mark style="color:orange;">标题：“考虑交易 Meta Platforms、Overstock.com、沃尔玛、家得宝或联合包裹服务的期权或股票吗？” 来源：PR Newswire 发布日期：2023 年 6 月 22 日 摘要：这篇新闻文章讨论了投资者可能考虑的潜在交易期权和股票，包括 Overstock.com。</mark>_

_<mark style="color:orange;">请注意，这些新闻文章仅供参考，可能不是价格变动的唯一原因。在做出任何投资决定之前，始终建议进行彻底的研究和分析。</mark>_

最后，您可以要求 ChatGPT 向 Airtable 添加新记录：

_<mark style="color:blue;">您可以使用股票代码、价格变动和新闻摘要向 Airtable 添加记录吗？</mark>_

_<mark style="color:orange;">我已经添加了一条包含以下详细信息的 Airtable 记录：</mark>_

_<mark style="color:orange;">股票代码：OSTK 价格变动：17.47% 新闻摘要：Overstock.com 成功竞标获得价值 2150 万美元的 Bed Bath & Beyond 资产后，其股价大幅上涨。</mark>_

[🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)**瞧！**这就是创建您自己的自定义工具并将其与 OpenAI 函数代理一起使用的方法！

## 附加说明

### 将会话 ID 传递给函数

默认情况下，自定义工具中的函数可以访问以下流程配置：

代码块 12json5
```json5
$flow.sessionId 
$flow.chatId
$flow.chatflowId
$flow.input
```
代码块 13

以下是将 sessionId 发送到 Discord webhook 的示例：

{% tabs %}
{% tab title="Javascript" %}
代码块 14javascript
```javascript
const fetch = require('node-fetch');
const webhookUrl = "https://discord.com/api/webhooks/1124783587267";
const content = $content; // 从输入模式中捕获
const sessionId = $flow.sessionId;

const body = {
	"content": `${mycontent} and the sessionid is ${sessionId}`
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json'
	},
	body: JSON.stringify(body)
};

const url = `${webhookUrl}?wait=true`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```
代码块 15
{% endtab %}
{% endtabs %}

### 将变量传递给函数

在某些情况下，您可能希望将变量传递给自定义工具函数。

例如，您正在创建一个使用自定义工具的聊天机器人。自定义工具正在执行 HTTP POST 调用，并且需要 API 密钥才能成功进行身份验证请求。您可以将其作为变量传递。

默认情况下，自定义工具中的函数可以访问变量：

```
$vars.<variable-name>
```

使用 API 和嵌入式在 Flowise 中传递变量的示例：

{% tabs %}
{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflow-id>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "vars": {
            "apiKey": "abc"
        }
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}

{% tab title="Embed" %}
```html
<script type="module">
    import Chatbot from 'https://cdn.jsdelivr.net/npm/flowise-embed/dist/web.js';
    Chatbot.init({
        chatflowid: 'chatflow-id',
        apiHost: 'http://localhost:3000',
        chatflowConfig: {
          vars: {
            apiKey: 'def'
          }
        }
    });
</script>
```
{% endtab %}
{% endtabs %}

在自定义工具中接收变量的示例：

{% tabs %}
{% tab title="Javascript" %}
```javascript
const fetch = require('node-fetch');
const webhookUrl = "https://discord.com/api/webhooks/1124783587267";
const content = $content; // 从输入模式中捕获
const sessionId = $flow.sessionId;
const apiKey = $vars.apiKey;

const body = {
	"content": `${mycontent} and the sessionid is ${sessionId}`
};

const options = {
	method: 'POST',
	headers: {
		'Content-Type': 'application/json',
		'Authorization': `Bearer ${apiKey}`
	},
	body: JSON.stringify(body)
};

const url = `${webhookUrl}?wait=true`

try {
	const response = await fetch(url, options);
	const text = await response.text();
	return text;
} catch (error) {
	console.error(error);
	return '';
}
```
{% endtab %}
{% endtabs %}

### 覆盖自定义工具

以下参数可以被覆盖

| 参数            | 描述            |
| --------------- | --------------- |
| `customToolName` | 工具名称        |
| `customToolDesc` | 工具描述        |
| `customToolSchema` | 工具模式        |
| `customToolFunc` | 工具函数        |

覆盖自定义工具参数的 API 调用的示例：

{% tabs %}
{% tab title="Javascript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflow-id>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "customToolName": "example_tool",
        "customToolSchema": "z.object({title: z.string()})"
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### 导入外部依赖项

您可以将任何内置的 NodeJS [模块](https://www.w3schools.com/nodejs/ref_modules.asp) 和支持的 [外部库](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/src/utils.ts#L289) 导入到函数中。

1. 要导入任何不受支持的库，您可以轻松地将新的 npm 包添加到 `packages/components` 文件夹中的 `package.json` 中。

```bash
cd Flowise && cd packages && cd components
pnpm add <your-library>
cd .. && cd ..
pnpm install
pnpm build
```

2. 然后，将导入的库添加到 `TOOL_FUNCTION_EXTERNAL_DEP` 环境变量中。参考


This translation maintains the original formatting and structure, replacing placeholder text like "replace with your api key" with more descriptive prompts.  It also ensures consistent terminology and phrasing throughout the document.
请参阅 [#builtin-and-external-dependencies](../../../configuration/environment-variables.md#builtin-and-external-dependencies "mention") 获取更多详细信息。

3. 启动应用

代码块 0 (bash)
```bash
pnpm start
```
代码块 1

4. 然后，您可以在 **JavaScript 函数** 中像这样使用新添加的库：

代码块 2 (javascript)
```javascript
const axios = require('axios')
```
代码块 3

观看如何添加其他依赖项和导入库的视频

{% embed url="https://youtu.be/0H1rrisc0ok" %}
