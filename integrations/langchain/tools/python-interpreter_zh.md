# E2B 的代码解释器(Code Interpreter)

[E2B](https://e2b.dev/) 是一个开源的运行时环境，用于在安全的云沙箱中执行 AI 生成的代码。例如，当用户请求生成数据的条形图时，LLM（大语言模型）将输出绘制图表所需的 Python 代码。生成的代码将被发送到 E2B，执行后的输出将包含图表的图像、代码、文本等。这些输出会被发送回 LLM 进行最终处理，然后显示在聊天中。

<figure><img src="../../../.gitbook/assets/image (176).png" alt=""><figcaption></figcaption></figure>