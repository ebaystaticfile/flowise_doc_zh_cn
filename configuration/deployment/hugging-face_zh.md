---
description: 如何在 Hugging Face 上部署 Flowise
---

# Hugging Face

***

### 创建新的空间

1. 登录 [Hugging Face](https://huggingface.co/login)
2. 开始创建一个 [新的空间](https://huggingface.co/new-space)，并为其命名。
3. 选择 **Docker** 作为 **Space SDK**，并选择 **Blank** 作为 Docker 模板。
4. 选择 **CPU basic ∙ 2 vCPU ∙ 16GB ∙ FREE** 作为 **空间硬件**。
5. 点击 **创建空间**。

### 设置环境变量

1. 前往新空间的 **设置**，找到 **变量和密钥** 部分。
2. 点击 **新建变量**，添加名为 `PORT`，值为 `7860` 的变量。
3. 点击 **保存**。
4. _(可选)_ 点击 **新建密钥**。
5. _(可选)_ 填写您的环境变量，例如数据库凭据、文件路径等。您可以在 [此处](https://github.com/FlowiseAI/Flowise/blob/main/docker/.env.example) 的 `.env.example` 文件中查看有效的字段。

### 创建 Dockerfile

1. 在文件选项卡中，点击 _**+ 添加文件**_ 按钮，然后点击 **创建新文件**（或者如果您愿意，也可以上传文件）。
2. 创建一个名为 **Dockerfile** 的文件，并粘贴以下内容：

代码块 0：Dockerfile
```dockerfile
FROM node:18-alpine
USER root

# 可以在构建时传递的参数
ARG FLOWISE_PATH=/usr/local/lib/node_modules/flowise
ARG BASE_PATH=/root/.flowise
ARG DATABASE_PATH=$BASE_PATH
ARG APIKEY_PATH=$BASE_PATH
ARG SECRETKEY_PATH=$BASE_PATH
ARG LOG_PATH=$BASE_PATH/logs
ARG BLOB_STORAGE_PATH=$BASE_PATH/storage

# 安装依赖项
RUN apk add --no-cache git python3 py3-pip make g++ build-base cairo-dev pango-dev chromium

ENV PUPPETEER_SKIP_DOWNLOAD=true
ENV PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser

# 全局安装 Flowise
RUN npm install -g flowise

# 使用 ARG 配置 Flowise 目录
RUN mkdir -p $LOG_PATH $FLOWISE_PATH/uploads && chmod -R 777 $LOG_PATH $FLOWISE_PATH

WORKDIR /data

CMD ["npx", "flowise", "start"]
```

3. 点击 **将文件提交到 `main`**，您的应用将开始构建。

### 完成 🎉

构建完成后，您可以点击 **应用** 选项卡查看您的正在运行的应用。
