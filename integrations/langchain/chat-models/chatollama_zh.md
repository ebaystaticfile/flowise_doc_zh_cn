# ChatOllama

## 先决条件

1. 下载 [Ollama](https://github.com/ollama/ollama) 或使用 [Docker](https://hub.docker.com/r/ollama/ollama) 运行。
2. 例如，您可以使用以下命令启动一个包含 llama3 的 Docker 实例：

    代码块 0 (bash)
    ```bash
    docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
    docker exec -it ollama ollama run llama3
    ```

## 设置

1. **聊天模型** > 拖动 **ChatOllama** 节点

<figure><img src="../../../.gitbook/assets/image (139).png" alt="" width="563"><figcaption></figcaption></figure>

2. 填写在 Ollama 上运行的模型。例如：`llama2`。您也可以使用其他参数：

<figure><img src="../../../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

3. 完成！🎉 现在您可以在 Flowise 中使用 **ChatOllama 节点** 了

<figure><img src="../../../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

### 附加说明

如果您同时在 Docker 中运行 Flowise 和 Ollama，则需要更改 ChatOllama 的基本 URL。

对于 Windows 和 MacOS 操作系统，请指定 [http://host.docker.internal:8000](http://host.docker.internal:8000/)。对于基于 Linux 的系统，由于 `host.docker.internal` 不可用，应使用默认的 Docker 网关： [http://172.17.0.1:8000](http://172.17.0.1:8000/)

<figure><img src="../../../.gitbook/assets/image (142).png" alt="" width="292"><figcaption></figcaption></figure>

## 资源

* [LangchainJS ChatOllama](https://js.langchain.com/docs/integrations/chat/ollama)
* [Ollama](https://github.com/ollama/ollama)
