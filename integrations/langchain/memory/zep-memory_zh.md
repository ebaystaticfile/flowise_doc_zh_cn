# Zep 内存

[Zep](https://github.com/getzep/zep) 是用于大型语言模型 (LLM) 应用程序的长期内存存储。它存储、总结、嵌入、索引和丰富 LLM 应用程序/聊天机器人历史记录，并通过简单、低延迟的 API 公开这些记录。

## 将 Zep 部署到 Render 的指南

您可以轻松地将 Zep 部署到 Render](https://render.com/)、[Flyio](https://fly.io/) 等云服务。如果您更喜欢在本地测试，也可以按照其[快速入门指南](https://github.com/getzep/zep#quick-start)启动一个 Docker 容器。

在此示例中，我们将部署到 Render。

1. 前往[Zep 代码库](https://github.com/getzep/zep#quick-start) 并单击**部署到 Render**
2. 这将带您进入 Render 的蓝图页面，只需单击**创建新资源**

<figure><img src="../../../.gitbook/assets/image (21) (1).png" alt=""><figcaption></figcaption></figure>

3. 部署完成后，您应该会看到在仪表板上创建了 3 个应用程序

<figure><img src="../../../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>

4. 只需单击第一个名为**zep**的应用程序并复制已部署的 URL

<figure><img src="../../../.gitbook/assets/image (38) (1).png" alt=""><figcaption></figcaption></figure>

## 将 Zep 部署到 Digital Ocean（通过 Docker）的指南

1. 克隆代码库

代码块 0bash
```bash
git clone https://github.com/getzep/zep.git
cd zep
nano .env
```

代码块 1

2. 在 .env 中添加您的 OpenAI API 密钥

代码块 2bash
```bash
ZEP_OPENAI_API_KEY=
```

代码块 3

代码块 4bash
```bash
docker compose up -d --build
```

代码块 5

3. 允许防火墙访问 8000 端口

代码块 6bash
```bash
sudo ufw allow from any to any port 8000 proto tcp
ufw status numbered
```

如果使用 Digital Ocean 的仪表板单独管理防火墙，请确保也添加了 8000 端口。


## 在 Flowise UI 中使用

1. 返回 Flowise 应用程序，只需创建一个新的画布或使用市场中的一个模板。在此示例中，我们将使用**简单的对话链**

<figure><img src="../../../.gitbook/assets/Untitled (3) (1).png" alt=""><figcaption></figcaption></figure>

2. 将**缓冲区内存**替换为**Zep 内存**。然后将**基本 URL**替换为您上面复制的 Zep URL

<figure><img src="../../../.gitbook/assets/Untitled (5).png" alt=""><figcaption></figcaption></figure>

3. 保存聊天流程并进行测试，查看是否记住了对话。

<figure><img src="../../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

4. 现在尝试清除聊天记录，您应该会看到它现在无法记住之前的对话。

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## Zep 身份验证

Zep 允许您使用 JWT 身份验证来保护您的实例。我们将在这里使用 `zepcli` 命令行实用程序 [here](https://github.com/getzep/zepcli/releases)。

#### 1. 生成密钥和 JWT 令牌 <a href="#id-1-generate-a-secret-and-the-jwt-token" id="id-1-generate-a-secret-and-the-jwt-token"></a>

下载 ZepCLI 后：

在 Linux 或 MacOS 上

```bash
./zepcli -i
```

在 Windows 上

```bash
zepcli.exe -i
```

您将首先获得您的密钥：

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

然后您将获得 JWT 令牌：

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### 2. 配置 Auth 环境变量 <a href="#id-2-configure-auth-environment-variables" id="id-2-configure-auth-environment-variables"></a>

在您的 Zep 服务器环境中设置以下环境变量：

```bash
ZEP_AUTH_REQUIRED=true
ZEP_AUTH_SECRET=<您上面生成的密钥>
```

#### 3. 在 Flowise 上配置凭据 <a href="#id-2-configure-auth-environment-variables" id="id-2-configure-auth-environment-variables"></a>

为 Zep 添加一个新的凭据，并将 JWT 令牌放入 API 密钥字段：

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

#### 4. 在 Zep 节点上使用创建的凭据 <a href="#id-2-configure-auth-environment-variables" id="id-2-configure-auth-environment-variables"></a>

在 Zep 节点连接凭据中，选择您刚刚创建的凭据。就是这样！

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
