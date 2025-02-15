# Chroma

## 先决条件

1. 下载并安装 [Docker](https://www.docker.com/) 和 [Git](https://git-scm.com/)
2. 使用终端克隆 [Chroma 的仓库](https://github.com/chroma-core/chroma)

代码块 0 (bash)
```bash
git clone https://github.com/chroma-core/chroma.git
```

3. 将目录路径更改为克隆的 Chroma 目录

代码块 1 (bash)
```bash
cd chroma
```

4. 运行 docker compose 来构建 Chroma 镜像和容器

代码块 2 (bash)
```bash
docker compose up -d --build
```

5. 如果成功，您将看到启动的 Docker 镜像：

<figure><img src="../../../.gitbook/assets/image (4) (1) (3).png" alt="" width="390"><figcaption></figcaption></figure>

## 设置

| 输入           | 描述                                                                                                                                        | 默认值               |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| 文档        | 可以连接到来自 [文档加载器](../document-loaders/) 的节点                                                                           |                       |
| 嵌入           | 可以连接到来自 [嵌入](../embeddings/) 的节点                                                                                      |                       |
| 集合名称       | Chroma 集合名称。有关命名约定，请参考 [此处](https://docs.trychroma.com/usage-guide#creating-inspecting-and-deleting-collections) |                       |
| Chroma URL     | 指定 Chroma 实例的 URL                                                                                                            | http://localhost:8000 |

<figure><img src="../../../.gitbook/assets/image (6) (1) (1) (1) (1) (2).png" alt="" width="238"><figcaption></figcaption></figure>

### 附加说明

如果您同时在 Docker 上运行 Flowise 和 Chroma，则需要执行其他步骤。

1. 首先启动 Chroma Docker

代码块 3 (bash)
```bash
docker compose up -d --build
```

2. 在 Flowise 中打开 `docker-compose.yml` 文件

```bash
cd Flowise && cd docker
```

3. 修改文件如下：

```sh
version: '3.1'

services:
    flowise:
        image: flowiseai/flowise
        restart: always
        environment:
            - PORT=${PORT}
            - FLOWISE_USERNAME=${FLOWISE_USERNAME}
            - FLOWISE_PASSWORD=${FLOWISE_PASSWORD}
            - DEBUG=${DEBUG}
            - DATABASE_PATH=${DATABASE_PATH}
            - APIKEY_PATH=${APIKEY_PATH}
            - SECRETKEY_PATH=${SECRETKEY_PATH}
            - FLOWISE_SECRETKEY_OVERWRITE=${FLOWISE_SECRETKEY_OVERWRITE}
            - LOG_PATH=${LOG_PATH}
            - LOG_LEVEL=${LOG_LEVEL}
            - EXECUTION_MODE=${EXECUTION_MODE}
        ports:
            - '${PORT}:${PORT}'
        volumes:
            - ~/.flowise:/root/.flowise
        networks:
            - flowise_net
        command: /bin/sh -c "sleep 3; flowise start"
networks:
    flowise_net:
        name: chroma_net
        external: true
```

4. 启动 Flowise Docker 镜像

```bash
docker compose up -d
```

5.  对于 Chroma URL，Windows 和 MacOS 操作系统请指定 [http://host.docker.internal:8000](http://host.docker.internal:8000/)。对于 Linux 系统，由于 `host.docker.internal` 不可用，应使用默认的 Docker 网关： [http://172.17.0.1:8000](http://172.17.0.1:8000/)

<figure><img src="../../../.gitbook/assets/image (5) (5).png" alt="" width="256"><figcaption></figcaption></figure>

## 资源

* [LangChain JS Chroma](https://js.langchain.com/docs/modules/indexes/vector_stores/integrations/chroma)
* [Chroma 快速入门](https://docs.trychroma.com/getting-started)
