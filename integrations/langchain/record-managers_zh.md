# 记录管理器(Record Managers)

***

记录管理器(Record Managers)用于跟踪已索引的文档，防止在[向量存储 Vector Store](vector-stores/)中出现重复的向量嵌入。

当文档分块进行更新插入(upsert)时，每个分块会使用[SHA-1](https://github.com/emn178/js-sha1)算法生成哈希值。这些哈希值将被存储在记录管理器中。如果存在相同的哈希值，则会跳过嵌入和更新插入过程。

在某些情况下，您可能需要删除与新索引文档同源的现有文档。为此，记录管理器提供了三种清理模式：

{% tabs %}
{% tab title="增量模式(Incremental)" %}
当需要更新插入多个文档，并希望保留现有文档中不属于当前更新插入流程的部分时，使用**增量模式(Incremental)**清理。

1. 假设我们有一个使用`增量模式`清理的记录管理器，并设置`source`作为源ID键(SourceId Key)

<div align="left"><figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="264"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="410"><figcaption></figcaption></figure></div>

2. 现有以下2个文档：

| 文本 | 元数据           |
| ---- | ---------------- |
| 猫   | `{source:"cat"}` |
| 狗   | `{source:"dog"}` |

<div align="left"><figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1).png" alt="" width="202"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (2).png" alt="" width="231"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. 更新插入后，可以看到2个文档被更新：

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2).png" alt="" width="433"><figcaption></figcaption></figure>

4. 现在如果删除**狗**文档，并将**猫**更新为**猫群**，结果如下：

<figure><img src="../../.gitbook/assets/image (13) (2).png" alt="" width="425"><figcaption></figcaption></figure>

* 原**猫**文档被删除
* 新增**猫群**文档
* **狗**文档保持不变
* 向量存储中剩余嵌入为**猫群**和**狗**

<figure><img src="../../.gitbook/assets/image (15) (1) (1).png" alt="" width="448"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="完全模式(Full)" %}
当更新插入多个文档时，**完全模式(Full)**清理会自动删除不属于当前更新插入流程的所有向量嵌入。

1. 假设我们有一个使用`完全模式`清理的记录管理器（无需设置源ID键）

<div align="left"><figure><img src="../../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="264"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (17) (1) (1).png" alt="" width="407"><figcaption></figcaption></figure></div>

2. 现有以下2个文档：

| 文本 | 元数据           |
| ---- | ---------------- |
| 猫   | `{source:"cat"}` |
| 狗   | `{source:"dog"}` |

<div align="left"><figure><img src="../../.gitbook/assets/image (11) (1) (1) (1) (1).png" alt="" width="202"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (10) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (2).png" alt="" width="231"><figcaption></figcaption></figure> <figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure></div>

3. 更新插入后，可以看到2个文档被更新：

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (2).png" alt="" width="433"><figcaption></figcaption></figure>

4. 现在如果删除**狗**文档，并将**猫**更新为**猫群**，结果如下：

<figure><img src="../../.gitbook/assets/image (18) (1) (1).png" alt="" width="430"><figcaption></figcaption></figure>

* 原**猫**文档被删除
* 新增**猫群**文档
* **狗**文档被删除
* 向量存储中仅剩**猫群**嵌入

<figure><img src="../../.gitbook/assets/image (19) (1) (1).png" alt="" width="527"><figcaption></figcaption></figure>
{% endtab %}

{% tab title="无(None)" %}
不执行任何清理操作
{% endtab %}
{% endtabs %}

当前可用的记录管理器节点：

* SQLite
* MySQL
* PostgreSQL

## 资源

* [LangChain 索引机制 - 工作原理](https://js.langchain.com/docs/how_to/indexing/#how-it-works) [中文 原英文](https://js.langchain.com/docs/how_to/indexing/#how-it-works)