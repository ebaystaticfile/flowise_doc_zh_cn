# 评估

{% hint style="info" %}
评估功能仅适用于云端和企业版计划
{% endhint %}

评估功能帮助您监控和理解您的 Chatflow/Agentflow 应用的性能。从宏观角度来看，评估是一个流程，它接收来自您的 Chatflow/Agentflow 的一组输入和相应的输出，并生成分数。这些分数可以通过将输出与参考结果进行比较来得出，例如通过字符串匹配、数值比较，甚至利用大型语言模型 (LLM) 作为评判标准。这些评估是使用数据集和评估器进行的。

## 数据集

数据集是用于运行您的 Chatflow/Agentflow 的输入，以及用于比较的相应输出。用户可以手动添加输入和预期输出，或者上传包含两列的 CSV 文件：输入和输出。

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

| 输入                             | 输出                       |
| --------------------------------- | ---------------------------- |
| 英国的首都是什么                 | 英国的首都是伦敦             |
| 一年有多少天                     | 一年有 365 天               |

## 评估器

评估器类似于单元测试。在评估过程中，来自数据集的输入将运行在选定的流程上，并使用选定的评估器评估输出。评估器有三种类型：

* **基于文本的**: 基于字符串的检查：
  * 包含任意一个
  * 包含所有
  * 不包含任意一个
  * 不包含所有
  * 以…开头
  * 不以…开头

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

* **基于数值的**: 数字类型检查：
  * 总令牌数
  * 提示令牌数
  * 完成令牌数
  * API 延迟
  * LLM 延迟
  * Chatflow 延迟
  * Agentflow 延迟 (即将推出)
  * 输出字符长度

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

* **基于LLM的**: 使用另一个LLM来评级输出
  * 幻觉
  * 正确性

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

## 评估流程

现在我们已经准备好了数据集和评估器，我们可以开始运行评估了。

1. 选择要评估的数据集和 Chatflow。您可以选择多个数据集和 Chatflow。使用下面的示例，数据集 1 中的每个输入都将针对 2 个 Chatflow 运行。由于数据集 1 有 2 个输入，因此将产生并评估总共 4 个输出。

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

2. 选择评估器。在此阶段，只能选择基于字符串和基于数值的评估器。

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

3. (可选) 选择基于 LLM 的评估器。启动评估：

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

4. 等待评估完成：

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

5. 评估完成后，单击右侧的图表图标以查看详细信息：

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

上面的三个图表显示了评估的摘要：

* 通过/失败率
* 使用的平均提示令牌和完成令牌
* 请求的平均延迟

图表下面的表格显示了每次执行的详细信息。

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (16).png" alt="" width="355"><figcaption></figcaption></figure>

### 重新运行评估

当评估中使用的流程已更新/修改时，将显示警告消息：

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

您可以使用右上角的“重新运行评估”按钮重新运行相同的评估。您将能够看到不同的版本：

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

您还可以查看和比较不同版本的成果：

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

## 视频教程

{% embed url="https://youtu.be/kgUttHMkGFg?si=3rLplEp_0TI0p6UV&t=486" %}
