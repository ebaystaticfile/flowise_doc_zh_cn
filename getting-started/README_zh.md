# 开始使用

***

## 云端部署

自行托管需要更多技术技能来设置实例、备份数据库和维护更新。如果您不擅长管理服务器，只想使用 Web 应用程序，我们建议您使用 [Flowise 云](https://flowiseai.com/join)。

## 快速入门

{% hint style="info" %}
先决条件：确保已在机器上安装 [NodeJS](https://nodejs.org/en/download)。支持 Node `v18.15.0` 或 `v20` 及更高版本。
{% endhint %}

使用 NPM 在本地安装 Flowise。

1. 安装 Flowise：

```bash
npm install -g flowise
```

您也可以安装特定版本。请参考可用的 [版本](https://www.npmjs.com/package/flowise?activeTab=versions)。

```
npm install -g flowise@x.x.x
```

2. 启动 Flowise：

```bash
npx flowise start
```

3. 打开： [http://localhost:3000](http://localhost:3000)

***

## Docker 部署

有两种方法可以使用 Docker 部署 Flowise：

### Docker Compose

1. 前往项目根目录下的 `docker` 文件夹
2. 复制 `.env.example` 文件，并将其粘贴为另一个名为 `.env` 的文件
3. 运行：

```bash
docker compose up -d
```

4. 打开： [http://localhost:3000](http://localhost:3000)
5. 您可以通过运行以下命令关闭容器：

```bash
docker compose stop
```

### Docker 镜像

1. 构建镜像：

```bash
docker build --no-cache -t flowise .
```

2. 运行镜像：

```bash
docker run -d --name flowise -p 3000:3000 flowise
```

3. 停止镜像：

```bash
docker stop flowise
```

***

## 开发者指南

Flowise 在单个单体存储库中包含 3 个不同的模块：

* **服务器端**: 提供 API 逻辑的 Node 后端
* **用户界面**: React 前端
* **组件**: 集成组件

### 先决条件

安装 [PNPM](https://pnpm.io/installation)。

```bash
npm i -g pnpm
```

### 设置 1

使用 PNPM 进行简单设置：

1. 克隆仓库

```bash
git clone https://github.com/FlowiseAI/Flowise.git
```

2. 进入仓库文件夹

```bash
cd Flowise
```

3. 安装所有模块的依赖项：

```bash
pnpm install
```

4. 构建代码：

```bash
pnpm build
```

在 [http://localhost:3000](http://localhost:3000) 启动应用程序

```bash
pnpm start
```

### 设置 2

项目贡献者的分步设置：

1. Fork 官方的 [Flowise Github 仓库](https://github.com/FlowiseAI/Flowise)
2. 克隆您已 fork 的仓库
3. 创建一个新分支，请参阅 [指南](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository)。命名约定：
   * 功能分支：`feature/<您的新功能>`
   * bug 修复分支：`bugfix/<您的新 bug 修复>`。
4. 切换到您刚刚创建的分支
5. 进入仓库文件夹：

```bash
cd Flowise
```

6. 安装所有模块的依赖项：

```bash
pnpm install
```

7. 构建代码：

```bash
pnpm build
```

8. 在 [http://localhost:3000](http://localhost:3000) 启动应用程序

```bash
pnpm start
```

9. 用于开发构建：

* 创建 `.env` 文件并在 `packages/ui` 中指定 `PORT`（参考 `.env.example`）
* 创建 `.env` 文件并在 `packages/server` 中指定 `PORT`（参考 `.env.example`）

```bash
pnpm dev
```

* 对 `packages/ui` 或 `packages/server` 做出的任何更改都将在 [http://localhost:8080](http://localhost:8080/) 反映出来
* 对于在 `packages/components` 中所做的更改，您需要重新构建才能获取更改
* 完成所有更改后，运行：

    ```bash
    pnpm build
    ```

    以及

    ```bash
    pnpm start
    ```

    以确保所有内容在生产环境中都能正常工作。

***

## 企业版

企业版计划拥有单独的仓库和 Docker 镜像。

获得对两者的访问权限后，设置方法与 [#setup-1](./#setup-1 "mention") 相同。在启动应用程序之前，企业用户需要在 `.env` 文件中填写企业参数的值。有关所需的更改，请参考 `.env.example`。

请联系 support@flowiseai.com 获取以下环境变量的值：

```
LICENSE_URL
FLOWISE_EE_LICENSE_KEY
```

对于 Docker 安装：

```bash
cd docker
cd enterprise
docker compose up -d
```

***

## 了解更多

在这个视频教程中，Leon 介绍了 Flowise，并解释了如何在本地机器上设置它。

{% embed url="https://youtu.be/nqAK_L66sIQ" %}

## 社区指南

* [使用 Flowise/LangChain 构建 LLM 应用程序的实践入门](https://volcano-ice-cd6.notion.site/Introduction-to-Practical-Building-LLM-Applications-with-Flowise-LangChain-03d6d75bfd20495d96dfdae964bea5a5)
* [Flowise / LangChainによるLLMアプリケーション構築\[実践\]入門](https://volcano-ice-cd6.notion.site/Flowise-LangChain-LLM-e106bb0f7e2241379aad8fa428ee064a)
