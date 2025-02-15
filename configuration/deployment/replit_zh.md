---
description: 在Replit上部署Flowise的步骤
---

# Replit

***

1. 登录[Replit](https://replit.com/~)
2. 创建一个新的**Repl**。选择**Node.js**作为模板，并填写您喜欢的**标题**。

<figure><img src="../../.gitbook/assets/image (18) (1) (2) (1).png" alt="" width="551"><figcaption></figcaption></figure>

3. 创建新的Repl后，在左侧边栏中点击“Secret”：

<figure><img src="../../.gitbook/assets/image (2) (4) (1).png" alt="" width="219"><figcaption></figcaption></figure>

4. 创建3个Secrets以跳过Puppeteer和Playwright库的Chromium下载。

<table><thead><tr><th width="403">Secrets</th><th>值</th></tr></thead><tbody><tr><td>PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD</td><td>1</td></tr><tr><td>PUPPETEER_SKIP_DOWNLOAD</td><td>true</td></tr><tr><td>PUPPETEER_SKIP_CHROMIUM_DOWNLOAD</td><td>true</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (5) (3).png" alt="" width="535"><figcaption></figcaption></figure>

5. 现在您可以切换到Shell选项卡

<figure><img src="../../.gitbook/assets/image (13) (2) (1).png" alt="" width="539"><figcaption></figcaption></figure>

6. 在Shell终端窗口中输入`npm install -g flowise`。如果您遇到关于Node版本不兼容的错误，请使用以下命令：`yarn global add flowise --ignore-engines`

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="530"><figcaption></figcaption></figure>

7. 然后输入`npx flowise start`

<figure><img src="../../.gitbook/assets/image (17) (1) (2).png" alt="" width="533"><figcaption></figcaption></figure>

8. 您现在应该能够在Replit上看到Flowise！

<figure><img src="../../.gitbook/assets/image (15) (3).png" alt="" width="545"><figcaption></figcaption></figure>

9. 如果您想启用[应用级授权](broken-reference/)，请将命令更改为：

代码块0bash
```bash
npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
```

10. 您现在将看到一个登录页面。只需使用您设置的用户名和密码登录即可。

<figure><img src="../../.gitbook/assets/image (12) (2) (1).png" alt=""><figcaption></figcaption></figure>
