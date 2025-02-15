## 在 Railway 上部署 Flowise

---

1. 点击以下预构建模板：[https://railway.app/template/pn4G8S?referralCode=WVNPD9](https://railway.app/template/pn4G8S?referralCode=WVNPD9)
2. 点击“立即部署”

<figure><img src="../../.gitbook/assets/image (1) (1) (2) (1).png" alt=""><figcaption></figcaption></figure>

3. 将名称更改为您首选的仓库名称，然后点击“部署”

<figure><img src="../../.gitbook/assets/image (2) (1) (2) (1).png" alt="" width="375"><figcaption></figcaption></figure>

4. 如果部署成功，您应该可以看到已部署的 URL。

<figure><img src="../../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

5. 要添加授权，请导航到“变量”选项卡并添加：

* `FLOWISE_USERNAME`
* `FLOWISE_PASSWORD`

<figure><img src="../../.gitbook/assets/image (15) (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

6.  您可以配置许多环境变量。请参考：[environment-variables.md](../environment-variables.md "mention")

搞定！您现在已在 Railway 上部署了 Flowise 🎉🎉 [https://emojipedia.org/party-popper/](https://emojipedia.org/party-popper/)


## 持久化卷

在 Railway 上运行的服务默认使用临时文件系统。Flowise 数据不会在部署和重启之间持久化。为了解决这个问题，我们可以使用 [Railway 卷](https://docs.railway.app/reference/volumes)。

为了简化步骤，我们提供了一个带有已挂载卷的 Railway 模板：[https://railway.app/template/nEGbjR](https://railway.app/template/nEGbjR)

只需点击“部署”并填写以下环境变量：

* `DATABASE_PATH` - `/opt/railway/.flowise`
* `APIKEY_PATH` - `/opt/railway/.flowise`
* `LOG_PATH` - `/opt/railway/.flowise/logs`
* `SECRETKEY_PATH` - `/opt/railway/.flowise`
* `BLOB_STORAGE_PATH` - `/opt/railway/.flowise/storage`

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="420"><figcaption></figcaption></figure>

现在尝试在 Flowise 中创建一个流程并保存它。然后尝试重启服务或重新部署，您应该仍然能够看到之前保存的流程。
