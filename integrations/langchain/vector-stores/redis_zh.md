# Redis

## 先决条件

1. 使用 Docker 启动一个 [Redis-Stack 服务器](https://redis.io/docs/latest/operate/oss_and_stack/install/install-stack/docker/)

```bash
docker run -d --name redis-stack-server -p 6379:6379 redis/redis-stack-server:latest
```

## 设置

1. 在画布上添加一个新的 **Redis** 节点。
2. 创建新的 Redis 凭据。

<figure><img src="../../../.gitbook/assets/image (1) (1) (3) (1) (1).png" alt="" width="257"><figcaption></figcaption></figure>

3. 选择 Redis 凭据类型。如果您有用户名和密码，请选择 Redis API；否则，请选择 Redis URL：

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (2).png" alt="" width="563"><figcaption></figcaption></figure>

4. 填写 URL：

<figure><img src="../../../.gitbook/assets/image (3) (1) (1) (1) (2) (1).png" alt="" width="542"><figcaption></figcaption></figure>

5. 现在您可以开始使用 Redis 更新数据了：

<figure><img src="../../../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (9) (2).png" alt=""><figcaption></figcaption></figure>

6. 导航到 Redis Insight 门户和您的数据库，您将能够看到所有已更新的数据：

<figure><img src="../../../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>
