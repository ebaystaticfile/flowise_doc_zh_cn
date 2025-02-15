# IBM Watsonx

## 先决条件

1. 在 [IBM Watsonx](https://www.ibm.com/watsonx) 注册账户。
2. 创建一个新项目：

<figure><img src="../../../.gitbook/assets/image (238).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (239).png" alt=""><figcaption></figcaption></figure>

3. 项目创建完成后，返回主仪表板，并点击**探索基础模型**：

<figure><img src="../../../.gitbook/assets/image (240).png" alt=""><figcaption></figcaption></figure>

4. 选择您想要使用的模型并在 Prompt Lab 中打开：

<figure><img src="../../../.gitbook/assets/image (241).png" alt=""><figcaption></figcaption></figure>

5. 点击右上角的“查看代码”：

<figure><img src="../../../.gitbook/assets/image (242).png" alt=""><figcaption></figcaption></figure>

6. 记录下 `model_id` 和 `version` 参数。在本例中，`model_id` 为 `ibm/granite-3-8b-instruct`，`version` 为 `2023-05-29`。
7. 点击左侧的导航栏，然后点击“开发者访问”：

<figure><img src="../../../.gitbook/assets/image (243).png" alt="" width="308"><figcaption></figcaption></figure>

8. 记录下 `watsonx.ai URL`、`项目 ID`，并在 IBM Cloud Console 中创建一个新的 API 密钥。
9. 现在，您应该拥有以下信息：
   * Watsonx.ai URL
   * 项目 ID
   * API 密钥
   * 模型版本
   * 模型 ID


## 设置

1. **聊天模型** > 拖动 **ChatIBMWatsonx** 节点

<figure><img src="../../../.gitbook/assets/image (244).png" alt="" width="306"><figcaption></figcaption></figure>

2. 使用前面获取的模型 ID 填充模型字段。创建新的凭据并填写所有详细信息。

<figure><img src="../../../.gitbook/assets/image (245).png" alt="" width="419"><figcaption></figcaption></figure>

3. 完成！🎉 现在您可以在 Flowise 中使用 **ChatIBMWatsonx 节点** 了！

<figure><img src="../../../.gitbook/assets/image (246).png" alt=""><figcaption></figcaption></figure>
