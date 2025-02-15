# 提取元数据检索器(Extract Metadata Retriever)

该检索器旨在自动从查询中提取关键词。提取的JSON输出将用作向量存储(vector store)的元数据过滤器(metadata filter)。

例如，当我们提出一个问题："苹果的利润是多少"，LLM（大语言模型）将输出 `{source: "apple"}`，并将其传递给向量存储的元数据过滤器。

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>