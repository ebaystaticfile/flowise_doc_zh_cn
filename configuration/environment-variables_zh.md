# 环境变量

Flowise 支持不同的环境变量来配置您的实例。您可以在 `packages/server` 文件夹内的 `.env` 文件中指定以下变量。请参考 [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/packages/server/.env.example) 文件。

<table><thead><tr><th width="233">变量</th><th width="219">描述</th><th width="104">类型</th><th>默认值</th></tr></thead><tbody><tr><td>PORT</td><td>Flowise 运行的 HTTP 端口</td><td>数字</td><td>3000</td></tr><tr><td>FLOWISE_USERNAME</td><td>登录用户名</td><td>字符串</td><td></td></tr><tr><td>FLOWISE_PASSWORD</td><td>登录密码</td><td>字符串</td><td></td></tr><tr><td>FLOWISE_FILE_SIZE_LIMIT</td><td>上传文件的最大大小</td><td>字符串</td><td><code>50mb</code></td></tr><tr><td>NUMBER_OF_PROXIES</td><td>速率限制代理</td><td>数字</td><td></td></tr><tr><td>CORS_ORIGINS</td><td>所有跨域 HTTP 调用的允许来源</td><td>字符串</td><td></td></tr><tr><td>IFRAME_ORIGINS</td><td>允许 iframe src 嵌入的来源</td><td>字符串</td><td></td></tr><tr><td>SHOW_COMMUNITY_NODES</td><td>显示社区创建的节点</td><td>布尔值：<code>true</code> 或 <code>false</code></td><td></td></tr><tr><td>DISABLED_NODES</td><td>要禁用的节点名称的逗号分隔列表</td><td>字符串</td><td></td></tr></tbody></table>

## 数据库配置

| 变量           | 描述                                                      | 类型                                       | 默认值                  |
| ------------------ | ---------------------------------------------------------------- | ------------------------------------------ | ------------------------ |
| DATABASE\_TYPE     | 用于存储 Flowise 数据的数据库类型                       | 枚举字符串：`sqlite`，`mysql`，`postgres` | `sqlite`                 |
| DATABASE\_PATH     | 数据库保存位置（当 DATABASE\_TYPE 为 sqlite 时）           | 字符串                                     | `your-home-dir/.flowise` |
| DATABASE\_HOST     | 主机 URL 或 IP 地址（当 DATABASE\_TYPE 不为 sqlite 时）     | 字符串                                     |                          |
| DATABASE\_PORT     | 数据库端口（当 DATABASE\_TYPE 不为 sqlite 时）              | 字符串                                     |                          |
| DATABASE\_USER     | 数据库用户名（当 DATABASE\_TYPE 不为 sqlite 时）            | 字符串                                     |                          |
| DATABASE\_PASSWORD | 数据库密码（当 DATABASE\_TYPE 不为 sqlite 时）            | 字符串                                     |                          |
| DATABASE\_NAME     | 数据库名称（当 DATABASE\_TYPE 不为 sqlite 时）              | 字符串                                     |                          |
| DATABASE\_SSL      | 是否需要数据库 SSL（当 DATABASE\_TYPE 不为 sqlite 时）     | 布尔值：`true` 或 `false`                 | `false`                  |

## 存储配置

Flowise 默认情况下将以下文件存储在本地路径文件夹下。

* 上传到 [文档加载器](../integrations/langchain/document-loaders/)/文档存储的文件
* 来自聊天的图像/音频上传
* 来自助手的图像/文件
* 来自 [向量上传 API](../using-flowise/api.md#vector-upsert-api) 的文件

用户可以指定 `STORAGE_TYPE` 来使用 AWS S3 或本地路径

<table><thead><tr><th width="227">变量</th><th width="196">描述</th><th width="131">类型</th><th>默认值</th></tr></thead><tbody><tr><td>STORAGE_TYPE</td><td>上传文件的存储类型，默认为 <code>local</code></td><td>枚举字符串：<code>s3</code>，<code>local</code></td><td><code>local</code></td></tr><tr><td>BLOB_STORAGE_PATH</td><td>当 <code>STORAGE_TYPE</code> 为 <code>local</code> 时，上传文件存储的本地文件夹路径</td><td>字符串</td><td><code>your-home-dir/.flowise/storage</code></td></tr><tr><td>S3_STORAGE_BUCKET_NAME</td><td>当 <code>STORAGE_TYPE</code> 为 <code>s3</code> 时，用于保存上传文件的存储桶名称</td><td>字符串</td><td></td></tr><tr><td>S3_STORAGE_ACCESS_KEY_ID</td><td>AWS 访问密钥</td><td>字符串</td><td></td></tr><tr><td>S3_STORAGE_SECRET_ACCESS_KEY</td><td>AWS 密钥</td><td>字符串</td><td></td></tr><tr><td>S3_STORAGE_REGION</td><td>S3 存储桶的区域</td><td>字符串</td><td></td></tr><tr><td>S3_ENDPOINT_URL</td><td>自定义 S3 端点（可选）</td><td>字符串</td><td></td></tr><tr><td>S3_FORCE_PATH_STYLE</td><td>强制使用 S3 路径样式（可选）</td><td>布尔值</td><td>false</td></tr></tbody></table>

## 调试和日志配置

| 变量   | 描述                         | 类型                                             |                                |
| ---------- | ----------------------------------- | ------------------------------------------------ | ------------------------------ |
| DEBUG      | 打印组件日志                         | 布尔值                                          |                                |
| LOG\_PATH  | 日志文件存储位置                     | 字符串                                           | `Flowise/packages/server/logs` |
| LOG\_LEVEL | 不同级别的日志                     | 枚举字符串：`error`，`info`，`verbose`，`debug` | `info`                         |

`DEBUG`：如果设置为 true，则会将日志打印到终端/控制台：

<figure><img src="../.gitbook/assets/image (3) (3).png" alt=""><figcaption></figcaption></figure>

`LOG_LEVEL`：要保存的日志记录器的不同日志级别。可以是 `error`、`info`、`verbose` 或 `debug`。默认设置为 `info`，只有 `logger.info` 将保存到日志文件中。如果您想要完整的详细信息，请设置为 `debug`。

<figure><img src="../.gitbook/assets/image (2) (4).png" alt=""><figcaption><p><strong>server-requests.log.jsonl - 记录发送到 Flowise 的每个请求</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p><strong>server.log - 记录 Flowise 上的一般操作</strong></p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5) (4).png" alt=""><figcaption><p><strong>server-error.log - 记录带有堆栈跟踪的错误</strong></p></figcaption></figure>

### 日志流式传输到 S3

当 `STORAGE_TYPE` 环境变量设置为 `s3` 时，日志将自动流式传输并存储到 S3。每小时将创建一个新的日志文件，从而更容易进行调试。

## 凭据配置

Flowise 使用加密密钥将您的第三方 API 密钥存储为加密凭据。

默认情况下，应用程序启动时会生成一个随机加密密钥，并将其存储在文件路径下。然后，每次都会检索此加密密钥来解密在聊天流中使用的凭据。例如，您的 OpenAI API 密钥、Pinecone API 密钥等。

您可以配置为使用 AWS Secret Manager 来存储加密密钥。

| 变量                      | 描述                                           | 类型                        | 默认值                   |
| ----------------------------- | ----------------------------------------------------- | --------------------------- | ------------------------- |
| SECRETKEY\_STORAGE\_TYPE      | 如何存储加密密钥                       | 枚举字符串：`local`，`aws` | `local`                   |
| SECRETKEY\_PATH               | 存储加密密钥的本地文件路径                     | 字符串                      | `Flowise/packages/server` |
| FLOWISE\_SECRETKEY\_OVERWRITE | 要使用的加密密钥，而不是现有密钥                 | 字符串                      |                           |
| SECRETKEY\_AWS\_ACCESS\_KEY   |                                                       | 字符串                      |                           |
| SECRETKEY\_AWS\_SECRET\_KEY   |                                                       | 字符串                      |                           |
| SECRETKEY\_AWS\_REGION        |                                                       | 字符串                      |                           |

由于某些原因，有时加密密钥可能会重新生成或存储路径已更改，这将导致类似以下错误 - <mark style="color:red;">无法解密凭据。</mark>

为了避免这种情况，您可以将自己的加密密钥设置为 `FLOWISE_SECRETKEY_OVERWRITE`，以便每次都使用相同的加密密钥。对格式没有限制，您可以将其设置为任何您想要的文本，或者与您的 `FLOWISE_PASSWORD` 相同。

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
UI 返回的凭据 API 密钥与您设置的原始 Api 密钥长度不同。这是一个虚假的前缀字符串，用于防止网络欺骗，这就是我们不将其返回到 UI 的原因。但是，在您与聊天流交互期间，将检索并使用正确的 Api 密钥。
{% endhint %}

## 模型配置

在某些情况下，您可能希望在现有的聊天模型和 LLM 节点上使用自定义模型，或者限制仅访问某些模型。

默认情况下，Flowise 从 [此处](https://github.com/FlowiseAI/Flowise/blob/main/packages/components/models.json) 获取模型列表。但是用户可以创建自己的 `models.json` 文件并指定文件路径：

<table><thead><tr><th width="164">变量</th><th width="196">描述</th><th width="78">类型</th><th>默认值</th></tr></thead><tbody><tr><td>MODEL_LIST_CONFIG_JSON</td><td>加载模型列表的链接，来自您的 <code>models.json</code> 配置文件</td><td>字符串</td><td><a href="https://raw.githubusercontent.com/FlowiseAI/Flowise/main/packages/components/models.json">https://raw.githubusercontent.com/FlowiseAI/Flowise/main/packages/components/models.json</a></td></tr></tbody></table>

## API 密钥配置

用户可以在 Flowise 中创建多个 API 密钥，以便使用 [API](../using-flowise/api_zh.md) 进行身份验证。默认情况下，密钥将作为 JSON 文件存储到您的本地文件路径。用户可以使用以下环境变量来更改此行为。

| 变量              | 描述                                                                                | 类型                      | 默认值                   |
| --------------------- | ------------------------------------------------------------------------------------------ | ------------------------- | ------------------------- |
| APIKEY\_STORAGE\_TYPE | 存储 API 密钥的方法                                                                   | 枚举字符串：`json`，`db` | `json`                    |
| APIKEY\_PATH          | 当 `APIKEY_STORAGE_TYPE` 未指定或为 `json` 时，API 密钥的存储位置                     | 字符串                    | `Flowise/packages/server` |

使用 `db` 作为存储类型会将 API 密钥存储到数据库中，而不是本地 JSON 文件。

<figure><img src="../.gitbook/assets/image (254).png" alt=""><figcaption><p>Flowise API 密钥</p></figcaption></figure>

## 内置和外部依赖项配置

Flowise 中的某些节点/功能允许用户运行 Javascript 代码。出于安全原因，默认情况下它只允许某些依赖项。可以通过设置以下环境变量来解除对内置和外部模块的限制：

| 变量                      | 描述                                          |        |
| ----------------------------- | ---------------------------------------------------- | ------ |
| TOOL\_FUNCTION\_BUILTIN\_DEP  | 用于工具函数的 NodeJS 内置模块                   | 字符串 |
| TOOL\_FUNCTION\_EXTERNAL\_DEP | 用于工具函数的外部模块                         | 字符串 |

{% code title=".env" %}
```bash
# 允许使用所有内置模块
TOOL_FUNCTION_BUILTIN_DEP=*

# 只允许使用 fs
TOOL_FUNCTION_BUILTIN_DEP=fs

# 只允许使用 crypto 和 fs
TOOL_FUNCTION_BUILTIN_DEP=crypto,fs

# 允许使用外部 npm 模块。
TOOL_FUNCTION_EXTERNAL_DEP=axios,moment
```
{% endcode %}

## 设置环境变量的示例：

### NPM

您可以使用 npx 运行 Flowise 时设置所有这些变量。例如：

```
npx flowise start --PORT=3000 --DEBUG=true
```

### Docker

```
docker run -d -p 5678:5678 flowise \
 -e DATABASE_TYPE=postgresdb \
 -e DATABASE_PORT=<POSTGRES_PORT> \
 -e DATABASE_HOST=<POSTGRES_HOST> \
 -e DATABASE_NAME=<POSTGRES_DATABASE_NAME> \
 -e DATABASE_USER=<POSTGRES_USER> \
 -e DATABASE_PASSWORD=<POSTGRES_PASSWORD> \
```

### Docker Compose

您可以在 `docker` 文件夹内的 `.env` 文件中设置所有这些变量。请参考 [.env.example](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example) 文件。
