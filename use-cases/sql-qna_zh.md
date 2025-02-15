# SQL 问答系统

***

与之前的示例，例如[网页抓取问答](web-scrape-qna_zh.md)和[多文档问答](multiple-documents-qna_zh.md)不同，查询结构化数据不需要向量数据库。高级别上，这可以通过以下步骤实现：

1. 向大型语言模型 (LLM) 提供：
   * SQL 数据库模式概述
   * 示例行数据
2. 使用少量样本提示返回 SQL 查询
3. 使用[If Else](../integrations/utilities/if-else_zh.md) 节点验证 SQL 查询
4. 创建一个自定义函数来执行 SQL 查询并获取响应
5. 从执行的 SQL 响应中返回自然语言响应

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

在这个示例中，我们将创建一个可以与存储在 SingleStore 中的 SQL 数据库交互的问答聊天机器人。

<figure><img src="../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

## TL;DR

您可以找到聊天流程模板：

{% file src="../.gitbook/assets/SQL Chatflow.json" %}

## 1. SQL 数据库模式 + 示例行

使用自定义 JS 函数节点连接到 SingleStore，检索数据库模式和前 3 行数据。

根据[研究论文](https://arxiv.org/abs/2204.00498) 的建议，建议使用以下示例格式生成提示：

```
CREATE TABLE samples (firstName varchar NOT NULL, lastName varchar)
SELECT * FROM samples LIMIT 3
firstName lastName
Stephen Tyler
Jack McGinnis
Steven Repici
```

<figure><img src="../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>完整 JavaScript 代码</summary>

```javascript
const HOST = 'singlestore-host.com';
const USER = 'admin';
const PASSWORD = 'mypassword';
const DATABASE = 'mydb';
const TABLE = 'samples';
const mysql = require('mysql2/promise');

let sqlSchemaPrompt;

function getSQLPrompt() {
  return new Promise(async (resolve, reject) => {
    try {
      const singleStoreConnection = mysql.createPool({
        host: HOST,
        user: USER,
        password: PASSWORD,
        database: DATABASE,
      });
  
      // 获取模式信息
      const [schemaInfo] = await singleStoreConnection.execute(
        `SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE table_name = "${TABLE}"`
      );
  
      const createColumns = [];
      const columnNames = [];
  
      for (const schemaData of schemaInfo) {
        columnNames.push(`${schemaData['COLUMN_NAME']}`);
        createColumns.push(`${schemaData['COLUMN_NAME']} ${schemaData['COLUMN_TYPE']} ${schemaData['IS_NULLABLE'] === 'NO' ? 'NOT NULL' : ''}`);
      }
  
      const sqlCreateTableQuery = `CREATE TABLE samples (${createColumns.join(', ')})`;
      const sqlSelectTableQuery = `SELECT * FROM samples LIMIT 3`;
  
      // 获取前 3 行
      const [rows] = await singleStoreConnection.execute(
          sqlSelectTableQuery,
      );
      
      const allValues = [];
      for (const row of rows) {
          const rowValues = [];
          for (const colName in row) {
              rowValues.push(row[colName]);
          }
          allValues.push(rowValues.join(' '));
      }
  
      sqlSchemaPrompt = sqlCreateTableQuery + '\n' + sqlSelectTableQuery + '\n' + columnNames.join(' ') + '\n' + allValues.join('\n');
      
      resolve();
    } catch (e) {
      console.error(e);
      return reject(e);
    }
  });
}

async function main() {
    await getSQLPrompt();
}

await main();

return sqlSchemaPrompt;
```

</details>

您可以从此[指南](broken-reference/)中了解如何获取`HOST`、`USER`、`PASSWORD`。完成后，单击执行：

<figure><img src="../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

现在我们可以看到已生成正确的格式。下一步是将其引入提示模板。

## 2. 使用少量样本提示返回 SQL 查询

创建一个新的聊天模型 + 提示模板 + LLM 链

<figure><img src="../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

在提示模板中指定以下提示：

```
根据提供的 SQL 表模式和下面的问题，返回一个可以回答用户问题的 SQL SELECT ALL 查询。例如：SELECT * FROM table WHERE id = '1'。
------------
SCHEMA: {schema}
------------
QUESTION: {question}
------------
SQL QUERY:
```

由于我们使用 2 个变量：{schema} 和 {question}，因此请在**格式化提示值**中指定它们的值：

<figure><img src="../.gitbook/assets/image (122).png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="info" %}
您可以向提示提供更多示例（即少量样本提示），以使 LLM 更好地学习。或者参考[特定方言提示](https://js.langchain.com/docs/use_cases/sql/prompting#dialect-specific-prompting)
{% endhint %}

## 3. 使用[If Else](../integrations/utilities/if-else_zh.md) 节点验证 SQL 查询

有时 SQL 查询无效，我们不想浪费资源去执行无效的 SQL 查询。例如，如果用户提出与 SQL 数据库无关的一般性问题。我们可以使用`If Else`节点路由到不同的路径。

例如，我们可以执行一个基本检查，查看 LLM 给出的 SQL 查询中是否包含 SELECT 和 WHERE。

{% tabs %}
{% tab title="If 函数" %}
```javascript
const sqlQuery = $sqlQuery.trim();

const regex = /SELECT\s.*?(?:\n|$)/gi;

// 提取 SQL 部分
const matches = sqlQuery.match(regex);
const cleanSql = matches ? matches[0].trim() : "";

if (cleanSql.includes("SELECT") && cleanSql.includes("WHERE")) {
    return cleanSql;
}
```
{% endtab %}

{% tab title="Else 函数" %}
```javascript
return $sqlQuery;
```
{% endtab %}
{% endtabs %}

<figure><img src="../.gitbook/assets/image (119).png" alt="" width="327"><figcaption></figcaption></figure>

在 Else 函数中，我们将路由到一个提示模板 + LLM 链，该链基本上告诉 LLM 它无法回答用户查询：

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

## 4. 执行 SQL 查询并获取响应的自定义函数

如果这是一个有效的 SQL 查询，我们需要执行该查询。将**If Else**节点的_**True**_输出连接到**自定义 JS 函数**节点：

<figure><img src="../.gitbook/assets/image (123).png" alt="" width="563"><figcaption></figcaption></figure>

<details>

<summary>完整 JavaScript 代码</summary>

```javascript
const HOST = 'singlestore-host.com';
const USER = 'admin';
const PASSWORD = 'mypassword';
const DATABASE = 'mydb';
const TABLE = 'samples';
const mysql = require('mysql2/promise');

let result;

function getSQLResult() {
  return new Promise(async (resolve, reject) => {
    try {
      const singleStoreConnection = mysql.createPool({
        host: HOST,
        user: USER,
        password: PASSWORD,
        database: DATABASE,
      });
     
      const [rows] = await singleStoreConnection.execute(
        $sqlQuery
      );
  
      result = JSON.stringify(rows)
      
      resolve();
    } catch (e) {
      console.error(e);
      return reject(e);
    }
  });
}

async function main() {
    await getSQLResult();
}

await main();

return result;
```

</details>

## 5. 从执行的 SQL 响应中返回自然语言响应

创建一个新的聊天模型 + 提示模板 + LLM 链

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

在提示模板中编写以下提示：

```
根据问题和 SQL 响应，编写尽可能详细的自然语言响应：
------------
QUESTION: {question}
------------
SQL RESPONSE: {sqlResponse}
------------
NATURAL LANGUAGE RESPONSE:
```

在**格式化提示值**中指定变量：

<figure><img src="../.gitbook/assets/image (125).png" alt="" width="563"><figcaption></figcaption></figure>

大功告成！您的 SQL 聊天机器人现在可以进行测试了！

## 查询

首先，让我们询问一些与数据库相关的内容。

<figure><img src="../.gitbook/assets/image (128).png" alt="" width="434"><figcaption></figcaption></figure>

查看日志，我们可以看到第一个 LLM 链能够给我们一个 SQL 查询：

**输入：**

{% code overflow="wrap" %}
```
根据提供的 SQL 表模式和下面的问题，返回一个可以回答用户问题的 SQL SELECT ALL 查询。例如：SELECT * FROM table WHERE id = '1'。
------------
SCHEMA: CREATE TABLE samples (id bigint(20) NOT NULL, firstName varchar(300) NOT NULL, lastName varchar(300) NOT NULL, userAddress varchar(300) NOT NULL, userState varchar(300) NOT NULL, userCode varchar(300) NOT NULL, userPostal varchar(300) NOT NULL, createdate timestamp(6) NOT NULL)
SELECT * FROM samples LIMIT 3
id firstName lastName userAddress userState userCode userPostal createdate
1125899906842627 Steven Repici 14 Kingston St. Oregon NJ 5578 Thu Dec 14 2023 13:06:17 GMT+0800 (Singapore Standard Time)
1125899906842625 John Doe 120 jefferson st. Riverside NJ 8075 Thu Dec 14 2023 13:04:32 GMT+0800 (Singapore Standard Time)
1125899906842629 Bert Jet 9th, at Terrace plc Desert City CO 8576 Thu Dec 14 2023 13:07:11 GMT+0800 (Singapore Standard Time)
------------
QUESTION: what is the address of John
------------
SQL QUERY:
```
{% endcode %}

**输出**

<pre class="language-sql"><code class="lang-sql"><strong>SELECT userAddress FROM samples WHERE firstName = 'John'
</strong></code></pre>

执行 SQL 查询后，结果将传递到第二个 LLM 链：

**输入**

{% code overflow="wrap" %}
```
根据问题和 SQL 响应，编写尽可能详细的自然语言响应：
------------
QUESTION: what is the address of John
------------
SQL RESPONSE: [{\"userAddress\":\"120 jefferson st.\"}]
------------
NATURAL LANGUAGE RESPONSE:
```
{% endcode %}

**输出**

```
John 的地址是 120 Jefferson St.
```

现在，如果我们询问与 SQL 数据库无关的内容，则将采用 Else 路由。

<figure><img src="../.gitbook/assets/image (132).png" alt="" width="428"><figcaption></figcaption></figure>

对于第一个 LLM 链，将生成如下 SQL 查询：

```sql
SELECT * FROM samples LIMIT 3
```

但是，它无法通过`If Else`检查，因为它不包含`SELECT`和`WHERE`，因此进入 Else 路由，该路由具有提示：

```
礼貌地说“我无法回答查询”
```

最终输出为：

```
对不起，我目前无法回答您的查询。
```

## 结论

在这个示例中，我们成功创建了一个可以与您的数据库交互的 SQL 聊天机器人，并且还能够处理与数据库无关的问题。进一步的改进包括添加内存以提供对话历史记录。

您可以在下面找到聊天流程：

{% file src="../.gitbook/assets/SQL Chatflow (1).json" %}
