---
description: Learn how to deploy Flowise on AWS
---

# 在 AWS 上部署 Flowise

***

## 前提条件

这需要您对 AWS 的基本工作原理有一定的了解。

在 AWS 上部署 Flowise 有两种方法：

* [使用 CloudFormation 部署到 ECS](aws.md#deploy-on-ecs-using-cloudformation)
* [手动配置 EC2 实例](aws.md#launch-ec2-instance)

## 使用 CloudFormation 部署到 ECS

CloudFormation 模板位于此处：[https://gist.github.com/MrHertal/549b31a18e350b69c7200ae8d26ed691](https://gist.github.com/MrHertal/549b31a18e350b69c7200ae8d26ed691)

它将 Flowise 部署到通过 ELB 暴露的 ECS 集群。

它受到此参考架构的启发：[https://github.com/aws-samples/ecs-refarch-cloudformation](https://github.com/aws-samples/ecs-refarch-cloudformation)

您可以随意编辑此模板以调整 Flowise 镜像版本、环境变量等。

使用 [AWS CLI](https://aws.amazon.com/fr/cli/) 部署 Flowise 的示例命令：

代码块0bash
aws cloudformation create-stack --stack-name flowise --template-body file://flowise-cloudformation.yml --capabilities CAPABILITY_IAM
代码块1

部署后，您的 Flowise 应用程序的 URL 将在 CloudFormation 堆栈输出中提供。

## 使用 Terraform 部署到 ECS

Terraform 文件（`variables.tf`、`main.tf`）位于此 GitHub 存储库中：[terraform-flowise-setup](https://github.com/huiseo/terraform-flowise-setup/tree/main)。

此设置将 Flowise 部署到通过应用程序负载均衡器 (ALB) 暴露的 ECS 集群。它基于 AWS 的 ECS 部署最佳实践。

您可以修改 Terraform 模板以调整：

* Flowise 镜像版本
* 环境变量
* 资源配置（CPU、内存等）

### 部署示例命令：

1. **初始化 Terraform：**

代码块2bash
terraform init
terraform apply
terraform destroy
代码块3

## 启动 EC2 实例

1. 在 EC2 仪表板中，单击**启动实例**

<figure><img src="../../.gitbook/assets/image (19) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

2. 向下滚动并**创建新的密钥对**（如果您没有密钥对）。

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

3. 填写您首选的密钥对名称。对于 Windows，我们将使用 `.ppk` 和 PuTTY 连接到实例。对于 Mac 和 Linux，我们将使用 `.pem` 和 OpenSSH。

<figure><img src="../../.gitbook/assets/image (15) (2) (1).png" alt="" width="370"><figcaption></figcaption></figure>

4. 单击**创建密钥对**并选择保存 `.ppk` 文件的位置路径。
5. 打开左侧边栏，从**安全组**中打开一个新选项卡。然后**创建安全组**。

<figure><img src="../../.gitbook/assets/image (20) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

6. 填写您首选的安全组名称和描述。接下来，将以下内容添加到入站规则并**创建安全组**。

<figure><img src="../../.gitbook/assets/image (12) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

7. 返回第一个选项卡（EC2 启动实例）并向下滚动到**网络设置**。选择您刚刚创建的安全组。

<figure><img src="../../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

8. 单击**启动实例**。几分钟后，导航回 EC2 仪表板，我们应该能够看到一个新的实例正在运行 [🎉](https://emojipedia.org/party-popper/)

<figure><img src="../../.gitbook/assets/image (17) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 如何连接到您的实例（Windows）

1. 对于 Windows，我们将使用 PuTTY。您可以从此处下载：[https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)。
2. 打开 PuTTY 并使用您的实例的公共 IPv4 DNS 名称填写**主机名**。

<figure><img src="../../.gitbook/assets/image (9) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. 在 PuTTY 配置的左侧边栏中，展开**SSH** 并单击**身份验证**。单击浏览并选择您之前下载的 `.ppk` 文件。

<figure><img src="../../.gitbook/assets/image (23) (1) (1).png" alt="" width="296"><figcaption></figcaption></figure>

4. 单击**打开**并**接受**弹出消息。

<figure><img src="../../.gitbook/assets/image (18) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

5. 然后以 `ec2-user` 用户身份登录。

<figure><img src="../../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

6. 现在您已连接到 EC2 实例。

## 如何连接到您的实例（Mac 和 Linux）

1. 在您的 Mac/Linux 上打开终端应用程序。
2. _(可选)_ 设置私钥文件的权限以限制对其的访问：

代码块4bash
chmod 400 /path/to/mykey.pem
代码块5

3. 使用 `ssh` 命令连接到您的 EC2 实例，指定用户名（`ec2-user`）、公共 IPv4 DNS 和 `.pem` 文件的路径。

代码块6bash
ssh -i /Users/username/Documents/mykey.pem ec2-user@ec2-123-45-678-910.compute-1.amazonaws.com
代码块7

4. 按 Enter 键，如果一切配置正确，您应该成功建立与 EC2 实例的 SSH 连接。

## 安装 Docker

1. 使用 yum 命令应用挂起的更新：

代码块8bash
sudo yum update
代码块9

2. 搜索 Docker 包：

代码块10bash
sudo yum search docker
代码块11

3. 获取版本信息：

代码块12bash
sudo yum info docker
代码块13

4. 安装 docker，运行：

代码块14bash
sudo yum install docker
代码块15

5. 添加默认 ec2-user 的组成员身份，以便您可以运行所有 docker 命令而无需使用 sudo 命令：

代码块16bash
sudo usermod -a -G docker ec2-user
id ec2-user
newgrp docker
代码块17

6. 安装 docker-compose：

代码块18bash
sudo yum install docker-compose-plugin
代码块19

7. 在 AMI 启动时启用 docker 服务：

代码块20bash
sudo systemctl enable docker.service
代码块21

8. 启动 Docker 服务：

代码块22bash
sudo systemctl start docker.service
代码块23

## 安装 Git

代码块24bash
sudo yum install git -y
代码块25

## 设置

1. 克隆仓库

代码块26bash
git clone https://github.com/FlowiseAI/Flowise.git
代码块27

2. 进入 docker 文件夹

代码块28bash
cd Flowise && cd docker
代码块29

3. 创建一个 `.env` 文件。您可以使用您喜欢的编辑器。我将使用 `nano`

代码块30bash
nano .env
```

<figure><img src="../../.gitbook/assets/image (13) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

4. 指定环境变量：

```sh
PORT=3000
DATABASE_PATH=/root/.flowise
APIKEY_PATH=/root/.flowise
SECRETKEY_PATH=/root/.flowise
LOG_PATH=/root/.flowise/logs
BLOB_STORAGE_PATH=/root/.flowise/storage
```

5. _(可选)_ 您还可以指定 `FLOWISE_USERNAME` 和 `FLOWISE_PASSWORD` 用于应用程序级别的授权。查看更多 [broken-reference](broken-reference/ "mention")
6. 然后按 `Ctrl + X` 退出，按 `Y` 保存文件。
7. 运行 docker compose

```bash
docker compose up -d
```

7. 您的应用程序现在已准备好通过您的公共 IPv4 DNS 在 3000 端口访问：

```
http://ec2-123-456-789.compute-1.amazonaws.com:3000
```

8. 您可以通过以下命令关闭应用程序：

```bash
docker compose stop
```

9. 您可以通过以下命令拉取最新镜像：

```bash
docker pull flowiseai/flowise
```

或者：

```bash
docker-compose pull
docker-compose up --build -d
```

## 使用 NGINX

如果您想去除 url 中的 :3000 并使用自定义域名，您可以使用 NGINX 将 80 端口反向代理到 3000 端口，以便用户可以使用您的域名打开应用程序。例如：`http://yourdomain.com`。

1. ```bash
   sudo yum install nginx
   ```
2. ```bash
   nginx -v
   ```
3. <pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl start nginx
   </strong></code></pre>
4. <pre class="language-bash"><code class="lang-bash"><strong>sudo nano /etc/nginx/conf.d/flowise.conf
   </strong></code></pre>
5. 复制粘贴以下内容并更改为您的域名：

```shell
server {
    listen 80;
    listen [::]:80;
    server_name yourdomain.com; #Example: demo.flowiseai.com
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

按 `Ctrl + X` 退出，按 `Y` 保存文件。

6. ```bash
   sudo systemctl restart nginx
   ```
7. 转到您的 DNS 提供商，并添加新的 A 记录。名称将是您的域名，值将是 EC2 实例的公共 IPv4 地址。

<figure><img src="../../.gitbook/assets/image (3) (2).png" alt="" width="367"><figcaption></figcaption></figure>

6. 您现在应该能够打开应用程序：`http://yourdomain.com`。

### 安装 Certbot 以获得 HTTPS

如果您希望您的应用程序具有 `https://yourdomain.com`。方法如下：

1. 为了在 NGINX 上安装 Certbot 并启用 HTTPS，我们将依赖 Python。因此，首先，让我们设置一个虚拟环境：

```bash
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

5. 完成证书生成向导后，我们将能够通过 HTTPS 使用地址 `https://yourdomain.com` 访问我们的 EC2 实例。

## 设置自动续订

要启用 Certbot 自动续订证书，只需通过运行以下命令添加一个 cron 作业：

```bash
echo "0 0,12 * * * root /opt/certbot/bin/python -c 'import random; import time; time.sleep(random.random() * 3600)' && sudo certbot renew -q" | sudo tee -a /etc/crontab > /dev/null
```

## 恭喜！

您已成功在 EC2 实例上设置 Flowise 应用程序，并在您的域名上拥有 SSL 证书 [🥳](https://emojipedia.org/partying-face/)
