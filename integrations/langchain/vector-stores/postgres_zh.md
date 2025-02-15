描述：>
 使用Postgres上的pgvector对嵌入式数据进行Upsert操作，并在查询时执行相似性搜索。
---

# Postgres

<figure><img src="../../../.gitbook/assets/image (163).png" alt="" width="292"><figcaption><p>Postgres节点</p></figcaption></figure>

连接到Postgres的方法有多种，具体取决于您的实例设置方式。下面是一个使用pgvector团队提供的预构建Docker镜像的本地配置示例。

创建一个名为`docker-compose.yml`的文件，内容如下：

```yaml
# 运行此命令启动数据库：
# docker-compose up --build
version: "3"
services:
  db:
    hostname: 127.0.0.1
    image: pgvector/pgvector:pg16
    ports:
      - 5432:5432
    restart: always
    environment:
      - POSTGRES_DB=api
      - POSTGRES_USER=myuser
      - POSTGRES_PASSWORD=ChangeMe
    volumes:
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
```

运行`docker compose up`启动Postgres容器。

使用配置的用户和密码创建新的凭据：

<figure><img src="../../../.gitbook/assets/image (50).png" alt="" width="526"><figcaption></figcaption></figure>

使用`docker-compose.yml`中配置的值填充节点字段。例如：

* 主机：**localhost**
* 数据库：**api**
* 端口：**5432**

搞定！您现在已成功设置好Postgres Vector，可以开始使用了。
