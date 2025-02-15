---
description: 学习如何在 Zeabur 上部署 Flowise
---

# Zeabur

***

{% hint style="warning" %}
请注意，以下 Zeabur 提供的模板已过期（2024年1月24日）。
{% endhint %}

1. 点击以下预构建[模板](https://zeabur.com/templates/2JYZTR)或下面的按钮。

[![在 Zeabur 上部署](https://zeabur.com/button.svg)](https://zeabur.com/templates/2JYZTR)

2. 点击“部署”

<figure><img src="../../.gitbook/assets/zeabur/1.png" alt="zeabur 模板"><figcaption></figcaption></figure>

3. 选择您喜欢的区域并继续

<figure><img src="../../.gitbook/assets/zeabur/2.png" alt="选择区域"><figcaption></figcaption></figure>

4. 您将被重定向到 Zeabur 的仪表板，您将看到部署过程

<figure><img src="../../.gitbook/assets/zeabur/3.png" alt="部署过程"><figcaption></figcaption></figure>

5. 要添加授权，请导航到“变量”选项卡并添加：

* FLOWISE\_USERNAME
* FLOWISE\_PASSWORD

<figure><img src="../../.gitbook/assets/zeabur/4.png" alt="授权"><figcaption></figcaption></figure>

6. 您可以配置一系列环境变量。请参考 [environment-variables.md](../environment-variables.md "提及")

就是这样！您现在已在 Zeabur 上部署了 Flowise [🎉](https://emojipedia.org/party-popper/)[🎉](https://emojipedia.org/party-popper/)

## 持久卷

Zeabur 将自动为您创建一个持久卷，因此您无需担心它。
