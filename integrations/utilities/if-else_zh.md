# 如果-否则

Flowise 允许您根据 if/else 条件将聊天流程拆分成不同的分支。

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>

### 输入变量

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

如上图所示，它接受任何具有 `json` 输出的节点。一些示例包括：自定义函数、LLM 链输出预测、获取/设置变量。

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

然后您可以指定一个变量名：

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure>

然后可以在 [If 函数](if-else.md#if-function) 和 [Else 函数](if-else.md#else-function) 中使用此变量，前缀为 `$`。例如：

代码块 0
$output
```

### 如果-否则 节点名称

您可以为节点命名，以便更轻松地可视化其功能。

### If 函数

这是在节点沙箱中运行的一段 JS 代码。它必须：

* 包含 `if` 语句
* 在 `if` 语句中返回一个值

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (2) (1).png" alt="" width="312"><figcaption></figcaption></figure>

这为用户提供了更大的灵活性，可以进行复杂的比较，例如正则表达式、日期比较等等。

### Else 函数

与 If 函数类似，它必须返回一个值。只有当 [If 函数](if-else.md#if-function) 没有返回值时，才会运行此函数。

<figure><img src="../../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1) (2) (1) (1).png" alt="" width="317"><figcaption></figcaption></figure>

### 输出

<figure><img src="../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

当 [If 函数](if-else.md#if-function) 成功返回一个值时，它将传递到如上图所示的 **True** 输出点。这允许用户将值传递到下一个节点。

否则，[Else 函数](if-else.md#else-function) 的返回值将传递到 **False** 输出点。

用户也可以在市场中查看 If Else 模板：

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>
