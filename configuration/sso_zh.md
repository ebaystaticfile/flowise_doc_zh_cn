# 单点登录 (SSO)

{% hint style="info" %}
SSO 功能仅适用于企业版计划
{% endhint %}

Flowise 支持 [OIDC](https://openid.net/)，允许用户使用单点登录 (SSO) 访问应用程序。目前，只有 [组织管理员](../using-flowise/workspaces.md#setting-up-admin-account) 可以配置 SSO 设置。

## Microsoft

1. 在 Azure 门户中，搜索 Microsoft Entra ID：

<figure><img src="../.gitbook/assets/image (193).png" alt=""><figcaption></figcaption></figure>

2. 在左侧栏中，单击“应用注册”，然后单击“新建注册”：

<figure><img src="../.gitbook/assets/image (194).png" alt=""><figcaption></figcaption></figure>

3. 输入应用名称，并选择“单租户”：

<figure><img src="../.gitbook/assets/image (195).png" alt=""><figcaption></figcaption></figure>

4. 创建应用后，记下应用程序 (客户端) ID 和目录 (租户) ID：

<figure><img src="../.gitbook/assets/image (196).png" alt=""><figcaption></figcaption></figure>

5. 在左侧栏中，单击“证书和机密” -> “新建客户端机密” -> “添加”：

<figure><img src="../.gitbook/assets/image (198).png" alt=""><figcaption></figcaption></figure>

6. 创建机密后，复制“值”，<mark style="color:red;">不要</mark>复制“机密 ID”：

<figure><img src="../.gitbook/assets/image (199).png" alt=""><figcaption></figcaption></figure>

7. 在左侧栏中，单击“身份验证” -> “添加平台” -> “Web”：

<figure><img src="../.gitbook/assets/image (201).png" alt=""><figcaption></figcaption></figure>

8. 填写重定向 URI。这需要根据您的托管方式进行更改：`http[s]://[your-flowise-instance.com]/api/v1/azure/callback`：

<figure><img src="../.gitbook/assets/image (218).png" alt="" width="514"><figcaption></figcaption></figure>

9. 您应该可以看到已创建的新重定向 URI：

<figure><img src="../.gitbook/assets/image (219).png" alt=""><figcaption></figcaption></figure>

10. 返回 Flowise 应用，以组织管理员身份登录。从左侧栏导航到 SSO 配置。填写步骤 4 中的 Azure 租户 ID 和客户端 ID，以及步骤 6 中的客户端机密。单击“测试配置”以查看连接是否可以成功建立：

<figure><img src="../.gitbook/assets/image (220).png" alt="" width="563"><figcaption></figcaption></figure>

11. 最后，启用并保存：

<figure><img src="../.gitbook/assets/image (221).png" alt="" width="563"><figcaption></figcaption></figure>

12. 在用户可以使用 SSO 登录之前，必须先邀请他们。请参阅 [邀请用户进行 SSO 登录](sso.md#inviting-users-for-sso-sign-in) 获取分步指南。受邀用户也必须是 Azure 中的目录用户。

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

## Google

要在您的网站上启用使用 Google 登录，您首先需要设置您的 Google API 客户端 ID。为此，请完成以下步骤：

1. 打开 [Google APIs 控制台](https://console.developers.google.com/apis) 的“凭据”页面。
2. 单击“创建凭据”>“OAuth 客户端 ID”

<figure><img src="../.gitbook/assets/image (224).png" alt="" width="563"><figcaption></figcaption></figure>

3. 选择“Web 应用程序”：

<figure><img src="../.gitbook/assets/image (225).png" alt="" width="504"><figcaption></figcaption></figure>

4. 填写重定向 URI。这需要根据您的托管方式进行更改：`http[s]://[your-flowise-instance.com]/api/v1/google/callback`：

<figure><img src="../.gitbook/assets/image (226).png" alt="" width="563"><figcaption></figcaption></figure>

5. 创建后，获取客户端 ID 和机密：

<figure><img src="../.gitbook/assets/image (227).png" alt=""><figcaption></figcaption></figure>

6. 返回 Flowise 应用，添加客户端 ID 和机密。测试连接并保存。

<figure><img src="../.gitbook/assets/image (228).png" alt="" width="563"><figcaption></figcaption></figure>

## Auth0

1. 在 [Auth0](https://auth0.com/) 注册一个帐户，然后创建一个新的应用程序

<figure><img src="../.gitbook/assets/image (229).png" alt=""><figcaption></figcaption></figure>

2. 选择“常规 Web 应用程序”：

<figure><img src="../.gitbook/assets/image (230).png" alt=""><figcaption></figcaption></figure>

3. 配置名称、描述等字段。记下**域**、**客户端 ID** 和**客户端机密**。

<figure><img src="../.gitbook/assets/image (231).png" alt=""><figcaption></figcaption></figure>

4. 填写应用程序 URI。这需要根据您的托管方式进行更改：`http[s]://[your-flowise-instance.com]/api/v1/auth0/callback`：

<figure><img src="../.gitbook/assets/image (232).png" alt=""><figcaption></figcaption></figure>

5. 在 API 选项卡中，确保已启用 Auth0 Management API 并具有以下权限：
   * read:users
   * read:client_grants

<figure><img src="../.gitbook/assets/image (233).png" alt=""><figcaption></figcaption></figure>

6. 返回 Flowise 应用，填写域、客户端 ID 和机密。测试并保存配置。

<figure><img src="../.gitbook/assets/image (234).png" alt="" width="563"><figcaption></figcaption></figure>

## 邀请用户进行 SSO 登录

为了让新用户能够使用 SSO 登录，我们必须将新用户邀请到 Flowise 应用程序中。这对于记录受邀用户的角色/工作区至关重要。有关环境变量配置，请参阅 [邀请用户](../using-flowise/workspaces.md#invite-user) 部分。

组织管理员可以选择受邀用户的登录类型：

<figure><img src="../.gitbook/assets/image (213).png" alt=""><figcaption></figcaption></figure>

* SSO：受邀用户只能使用 SSO 登录
* 电子邮件/密码：受邀用户只能通过电子邮件/密码登录

受邀用户将收到登录邀请链接：

<figure><img src="../.gitbook/assets/image (222).png" alt="" width="449"><figcaption></figcaption></figure>

单击该按钮会将受邀用户直接带到 Flowise SSO 登录屏幕：

<figure><img src="../.gitbook/assets/image (210).png" alt="" width="400"><figcaption></figcaption></figure>

或者导航到 Flowise 应用并使用 SSO 登录：

<figure><img src="../.gitbook/assets/image (211).png" alt="" width="437"><figcaption></figcaption></figure>
