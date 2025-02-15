# SingleStore 设置

1. 在 [SingleStore](https://www.singlestore.com/) 注册账户。
2. 登录门户网站。在左侧面板中，点击 **云 (CLOUD)** -> **创建新的工作区组 (Create new workspace group)**。然后点击 **创建工作区 (Create Workspace)** 按钮。

<figure><img src="../../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

3. 选择云提供商和数据区域，然后点击 **下一步 (Next)**：

<figure><img src="../../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

4. 查看并点击 **创建工作区 (Create Workspace)**：

<figure><img src="../../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

5. 您现在应该可以看到已创建的工作区：

<figure><img src="../../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

6. 继续创建数据库：

<figure><img src="../../../.gitbook/assets/image (65).png" alt="" width="485"><figcaption></figcaption></figure>

7. 您应该可以看到已创建的数据库并已附加到工作区：

<figure><img src="../../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

8. 点击工作区下拉菜单中的 **连接 (Connect)** -> **直接连接 (Connect Directly)**：

<figure><img src="../../../.gitbook/assets/image (61).png" alt="" width="418"><figcaption></figcaption></figure>

9. 您可以指定一个新密码或使用默认生成的密码。然后点击 **继续 (Continue)**：

<figure><img src="../../../.gitbook/assets/image (62).png" alt="" width="485"><figcaption></figcaption></figure>

10. 在选项卡中，切换到 **您的应用 (Your App)**，然后从下拉菜单中选择 **Node.js**。请记下/保存 `用户名 (Username)`、`主机 (Host)`、`密码 (Password)`，因为您稍后会在 Flowise 中需要这些信息。

<figure><img src="../../../.gitbook/assets/image (63).png" alt="" width="563"><figcaption></figcaption></figure>

11. 返回 Flowise 画布，拖放 SingleStore 节点。点击凭据下拉菜单中的 **创建新的 (Create New)**：

<figure><img src="../../../.gitbook/assets/image (4) (1) (2) (1).png" alt="" width="271"><figcaption></figcaption></figure>

12. 输入上述用户名和密码：

<figure><img src="../../../.gitbook/assets/image (64).png" alt="" width="563"><figcaption></figcaption></figure>

13. 然后指定主机和数据库名称：

<figure><img src="../../../.gitbook/assets/image (5) (1) (2).png" alt="" width="272"><figcaption></figcaption></figure>

14. 现在您可以开始使用 SingleStore 执行 upsert 操作了：

<figure><img src="../../../.gitbook/assets/image (6) (1) (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (7) (1) (2).png" alt=""><figcaption></figcaption></figure>

15. 返回 SingleStore 门户网站和您的数据库，您将能够看到所有已执行 upsert 操作的数据：

<figure><img src="../../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>
