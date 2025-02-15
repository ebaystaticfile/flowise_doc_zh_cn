---
description: 学习如何为您的 Flowise 实例设置应用级访问控制
---

# 应用级访问控制

***

应用级授权通过用户名和密码保护您的 Flowise 实例。这可以防止您的应用在在线部署时被任何人访问。

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 如何设置用户名和密码

### npm 方法

1. 安装 Flowise

```bash
npm install -g flowise
```

2. 使用用户名和密码启动 Flowise

```bash
npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
```

3. 打开 [http://localhost:3000](http://localhost:3000)


### Docker 方法

1. 导航到 `docker` 文件夹

```
cd docker
```

2. 创建 `.env` 文件并指定 `PORT`、`FLOWISE_USERNAME` 和 `FLOWISE_PASSWORD`

```sh
PORT=3000
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```

3. 将 `FLOWISE_USERNAME` 和 `FLOWISE_PASSWORD` 传递到 `docker-compose.yml` 文件：

```yaml
environment:
    - PORT=${PORT}
    - FLOWISE_USERNAME=${FLOWISE_USERNAME}
    - FLOWISE_PASSWORD=${FLOWISE_PASSWORD}
```

4. 执行 `docker compose up -d`
5. 打开 [http://localhost:3000](http://localhost:3000)
6.  可以使用 `docker compose stop` 关闭容器


### Git clone 方法

要启用应用级身份验证，请将 `FLOWISE_USERNAME` 和 `FLOWISE_PASSWORD` 添加到 `packages/server` 文件夹中的 `.env` 文件：

```
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```
