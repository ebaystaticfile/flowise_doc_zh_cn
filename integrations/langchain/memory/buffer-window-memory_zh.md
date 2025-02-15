# 缓冲窗口内存

使用 Flowise 数据库表 `chat_message` 作为存储/检索对话的机制。

不同之处在于它只获取最后 K 次交互。这种方法有利于保留最近交互的滑动窗口，确保缓冲区大小保持可管理。

<figure><img src="../../../.gitbook/assets/image (1) (1) (3) (1).png" alt="" width="298"><figcaption></figcaption></figure>

## 输入参数

| 参数名      | 描述                                                                       | 默认值       |
|-------------|-----------------------------------------------------------------------------|---------------|
| Size        | 获取最后 K 条消息                                                             | 4             |
| Session Id  | 用于检索/存储消息的 ID。如果未指定，则使用随机 ID。                            |               |
| Memory Key  | 用于在提示模板中格式化消息的键                                               | chat\_history |
