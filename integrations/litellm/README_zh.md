description: Learn how Flowise integrates with LiteLLM Proxy

# LiteLLM 代理

使用 [LiteLLM 代理](https://docs.litellm.ai/docs/simple_proxy) 与 Flowise 集成，以实现：

- Azure OpenAI/LLM 端点负载均衡
- 调用 100 多个 OpenAI 格式的 LLM
- 使用虚拟密钥设置预算、速率限制和跟踪使用情况

## 如何在 Flowise 中使用 LiteLLM 代理

### 步骤 1：在 LiteLLM 的 `config.yaml` 文件中定义您的 LLM 模型

LiteLLM 需要一个包含所有已定义模型的配置文件，我们将此文件命名为 `litellm_config.yaml`

[有关如何设置 litellm 配置的详细文档 - 点击此处](https://docs.litellm.ai/docs/proxy/configs)

代码块 0 (YAML)
```yaml
model_list:
  - model_name: gpt-4
    litellm_params:
      model: azure/chatgpt-v-2
      api_base: https://openai-gpt-4-test-v-1.openai.azure.com/
      api_version: "2023-05-15"
      api_key: 
  - model_name: gpt-4
    litellm_params:
      model: azure/gpt-4
      api_key: 
      api_base: https://openai-gpt-4-test-v-2.openai.azure.com/
  - model_name: gpt-4
    litellm_params:
      model: azure/gpt-4
      api_key: 
      api_base: https://openai-gpt-4-test-v-2.openai.azure.com/
```
代码块 1


### 步骤 2：启动 LiteLLM 代理

```shell
docker run \
    -v $(pwd)/litellm_config.yaml:/app/config.yaml \
    -p 4000:4000 \
    ghcr.io/berriai/litellm:main-latest \
    --config /app/config.yaml --detailed_debug
```

启动成功后，代理将在 `http://localhost:4000/` 运行。


### 步骤 3：在 Flowise 中使用 LiteLLM 代理

在 Flowise 中，指定 **标准 OpenAI 节点（而非 Azure OpenAI 节点）** —— 这适用于 **聊天模型、嵌入、LLM——所有内容**

- 将 `BasePath` 设置为 LiteLLM 代理 URL（本地运行时为 `http://localhost:4000`）
- 设置以下标头 `Authorization: Bearer <your-litellm-master-key>`
