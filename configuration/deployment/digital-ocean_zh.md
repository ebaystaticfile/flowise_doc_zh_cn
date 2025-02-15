# 在 Digital Ocean 上部署 Flowise

***

## 创建 Droplet

本节将指导您创建 Droplet。更多信息，请参考[官方指南](https://docs.digitalocean.com/products/droplets/quickstart/)。

1. 首先，点击下拉菜单中的**Droplets**

<figure><img src="../../.gitbook/assets/image (15) (2).png" alt=""><figcaption></figcaption></figure>

2. 选择数据区域和一个基础的 6 美元/月的 Droplet 类型

<figure><img src="../../.gitbook/assets/image (17) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. 选择身份验证方法。在本例中，我们将使用密码。

<figure><img src="../../.gitbook/assets/image (5) (2).png" alt=""><figcaption></figcaption></figure>

4. 一段时间后，您应该可以看到您的 Droplet 已成功创建。

<figure><img src="../../.gitbook/assets/image (7) (2) (1).png" alt=""><figcaption></figcaption></figure>


## 如何连接到您的 Droplet

Windows 用户请遵循此[指南](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/putty/)。

Mac/Linux 用户请遵循此[指南](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/openssh/)。


## 安装 Docker

1.  执行以下命令：
    ```bash
    curl -fsSL https://get.docker.com -o get-docker.sh
    ```
2.  执行以下命令：
    ```bash
    sudo sh get-docker.sh
    ```
3. 安装 docker-compose：
    ```bash
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    ```
4. 设置权限：
    ```bash
    sudo chmod +x /usr/local/bin/docker-compose
    ```


## 设置

1. 克隆仓库：
    ```bash
    git clone https://github.com/FlowiseAI/Flowise.git
    ```
2. 进入 docker 文件夹：
    ```bash
    cd Flowise && cd docker
    ```
3. 创建一个 `.env` 文件。您可以使用您喜欢的编辑器。这里我们将使用 `nano`：
    ```bash
    nano .env
    ```

<figure><img src="../../.gitbook/assets/image (10) (2).png" alt="" width="375"><figcaption></figcaption></figure>

4. 指定环境变量：
    ```bash
    PORT=3000
    DATABASE_PATH=/root/.flowise
    APIKEY_PATH=/root/.flowise
    SECRETKEY_PATH=/root/.flowise
    LOG_PATH=/root/.flowise/logs
    BLOB_STORAGE_PATH=/root/.flowise/storage
    ```

5. (可选) 您也可以指定 `FLOWISE_USERNAME` 和 `FLOWISE_PASSWORD` 用于应用程序级别的授权。更多信息请参考[失效链接](broken-reference/)

6. 按 `Ctrl + X` 退出，按 `Y` 保存文件。

7. 运行 docker compose：
    ```bash
    docker compose up -d
    ```

8. 然后您可以访问应用程序： "您的公共 IPv4 DNS":3000。例如：`176.63.19.226:3000`

9. 您可以通过以下命令停止应用程序：
    ```bash
    docker compose stop
    ```

10. 您可以通过以下命令拉取最新镜像：
    ```bash
    docker pull flowiseai/flowise
    ```


## 添加反向代理和 SSL

反向代理是将应用程序服务器暴露给互联网的推荐方法。它允许我们仅使用 URL 而不是服务器 IP 和端口号来连接到我们的 Droplet。这提供了安全优势，例如将应用程序服务器与直接互联网访问隔离，能够集中防火墙保护，减少常见威胁（如拒绝服务攻击）的攻击面，最重要的是，能够在一个地方终止 SSL/TLS 加密。

> 如果您的 Droplet 没有 SSL，则嵌入式小部件和 API 端点在现代浏览器中将无法访问。这是因为浏览器已经开始弃用 HTTP 而支持 HTTPS，并阻止来自通过 HTTPS 加载的页面的 HTTP 请求。


### 步骤 1 — 安装 Nginx

1. Nginx 可通过默认存储库使用 apt 安装。更新您的存储库索引，然后安装 Nginx：

    ```bash
    sudo apt update
    sudo apt install nginx
    ```

    > 按 Y 确认安装。如果系统要求您重新启动服务，请按 ENTER 接受默认设置。

2. 您需要允许防火墙访问 Nginx。根据初始服务器先决条件设置服务器后，使用 ufw 添加以下规则：

    ```bash
    sudo ufw allow 'Nginx HTTP'
    ```

3. 现在您可以验证 Nginx 是否正在运行：

    ```bash
    systemctl status nginx
    ```

    输出：

    ```bash
    ● nginx.service - A high performance web server and a reverse proxy server
         Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
         Active: active (running) since Mon 2022-08-29 06:52:46 UTC; 39min ago
           Docs: man:nginx(8)
       Main PID: 9919 (nginx)
          Tasks: 2 (limit: 2327)
         Memory: 2.9M
            CPU: 50ms
         CGroup: /system.slice/nginx.service
                 ├─9919 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
                 └─9920 "nginx: worker process
    ```

接下来，您将添加一个包含您的域名和应用程序服务器代理的自定义服务器块。


### 步骤 2 — 配置服务器块 + DNS 记录

建议的做法是为新的服务器块添加内容创建自定义配置文件，而不是直接编辑默认配置。

1. 使用 nano 或您首选的文本编辑器创建并打开一个新的 Nginx 配置文件：

```bash
sudo nano /etc/nginx/sites-available/your_domain
```

2. 将以下内容插入您的新文件，确保将 `your_domain` 替换为您自己的域名：

```
server {
    listen 80;
    listen [::]:80;
    server_name your_domain; # 例如：demo.flowiseai.com
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_cache_bypass $http_upgrade;
    }
}
```

3. 使用 `nano`，您可以通过按 `CTRL+O` 然后 `CTRL+X` 来保存并退出。

4. 接下来，通过从此文件创建到 Nginx 在启动时读取的 sites-enabled 目录的链接来启用此配置文件，再次确保将 `your_domain` 替换为您自己的域名：

```bash
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
```

5. 现在您可以测试您的配置文件是否存在语法错误：

```bash
sudo nginx -t
```

6. 如果没有报告问题，请重新启动 Nginx 以应用您的更改：

```bash
sudo systemctl restart nginx
```

7. 前往您的 DNS 提供商，并添加一个新的 A 记录。名称将是您的域名，值将是您 Droplet 的公共 IPv4 地址。

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt="" width="367"><figcaption></figcaption></figure>

Nginx 现在配置为您的应用程序服务器的反向代理。您现在应该能够打开应用程序：http://yourdomain.com。


### 步骤 3 — 安装 Certbot 用于 HTTPS (SSL)

如果您想为您的 Droplet 添加安全的 `https` 连接，例如 https://yourdomain.com，您需要执行以下操作：

1. 为了安装 Certbot 并启用 NGINX 上的 HTTPS，我们将依赖 Python。因此，首先，让我们设置一个虚拟环境：

```bash
apt install python3.10-venv
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip
```

2. 之后，运行以下命令安装 Certbot：

```bash
sudo /opt/certbot/bin/pip install certbot certbot-nginx
```

3. 现在，执行以下命令以确保可以运行 `certbot` 命令：

```bash
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```

4. 最后，运行以下命令以获取证书并让 Certbot 自动修改 NGINX 配置，启用 HTTPS：

```bash
sudo certbot --nginx
```

5. 完成证书生成向导后，我们将能够通过 HTTPS 使用地址 https://yourdomain.com 访问我们的 Droplet。


### 设置自动续订

要启用 Certbot 自动续订证书，只需通过运行以下命令添加一个 cron 作业：

```bash
echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```


## 恭喜！

您已成功在您的 Droplet 上设置 Flowise，并在您的域名上安装了 SSL 证书 [🥳](https://emojipedia.org/partying-face/)


## 在 Digital Ocean 上更新 Flowise 的步骤

1. 导航到您安装 Flowise 的目录：

```bash
cd Flowise/docker
```

2. 停止并删除 docker 镜像

注意：这不会删除您的流程，因为数据库存储在单独的文件夹中。

```bash
sudo docker compose stop
sudo docker compose rm
```

3. 拉取最新的 Flowise 镜像

您可以在[此处](https://github.com/FlowiseAI/Flowise/releases)查看最新的版本发布。

```bash
docker pull flowiseai/flowise
```

4. 启动 docker

```bash
docker compose up -d
```
