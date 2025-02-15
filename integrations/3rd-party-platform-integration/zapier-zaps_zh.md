---
description: Learn how to integrate Flowise and Zapier
---

# Zapier Zaps

***

## Prerequisites

1. Log in to Zapier: [https://zapier.com/app/login](https://zapier.com/app/login) or sign up: [https://zapier.com/sign-up](https://zapier.com/sign-up)
2. Deploy a cloud-hosted version of Flowise (see [deployment instructions](../../configuration/deployment/)).

## Setup

1. Go to Zapier Zaps: [https://zapier.com/app/zaps](https://zapier.com/app/zaps)
2. Click **Create**.

<figure><img src="../../.gitbook/assets/zapier/zap/1.png" alt=""><figcaption></figcaption></figure>

### Receiving Trigger Messages

1. Click or search for **Discord**.

    <figure><img src="../../.gitbook/assets/zapier/zap/2.png" alt="" width="563"><figcaption></figcaption></figure>
2. Select the **New Message Posted to Channel** event and click **Continue**.

    <figure><img src="../../.gitbook/assets/zapier/zap/3.png" alt="" width="563"><figcaption></figcaption></figure>
3. Sign in to your Discord account.

    <figure><img src="../../.gitbook/assets/zapier/zap/4.png" alt="" width="563"><figcaption></figcaption></figure>
4. Add the **Zapier Bot** to your desired server.

    <figure><img src="../../.gitbook/assets/zapier/zap/5.png" alt="" width="272"><figcaption></figcaption></figure>
5. Grant the necessary permissions, click **Authorize**, and then click **Continue**.

    <figure><img src="../../.gitbook/assets/zapier/zap/6.png" alt="" width="292"><figcaption></figcaption></figure>

    <figure><img src="../../.gitbook/assets/zapier/zap/7.png" alt="" width="290"><figcaption></figcaption></figure>
6. Select your desired channel to interact with the Zapier Bot and click **Continue**.

    <figure><img src="../../.gitbook/assets/zapier/zap/8.png" alt="" width="563"><figcaption></figcaption></figure>
7. Send a test message to the selected channel (from step 6).

    <figure><img src="../../.gitbook/assets/zapier/zap/9.png" alt="" width="563"><figcaption></figcaption></figure>
8. Click **Test trigger**.

    <figure><img src="../../.gitbook/assets/zapier/zap/10.png" alt="" width="563"><figcaption></figcaption></figure>
9. Select your message and click **Continue with the selected record**.

    <figure><img src="../../.gitbook/assets/zapier/zap/11.png" alt="" width="563"><figcaption></figcaption></figure>

### Filtering Out Zapier Bot Messages

1. Click or search for **Filter**.

    <figure><img src="../../.gitbook/assets/zapier/zap/12.png" alt="" width="563"><figcaption></figcaption></figure>
2. Configure the **Filter** to prevent continuation if the message is from the **Zapier Bot**, then click **Continue**.

    <figure><img src="../../.gitbook/assets/zapier/zap/13.png" alt="" width="563"><figcaption></figcaption></figure>

### FlowiseAI Result Generation

1. Click **+**, then click or search for **FlowiseAI**.

    <figure><img src="../../.gitbook/assets/zapier/zap/14.png" alt="" width="563"><figcaption></figcaption></figure>
2. Select the **Make Prediction** event and click **Continue**.

    <figure><img src="../../.gitbook/assets/zapier/zap/15.png" alt="" width="563"><figcaption></figcaption></figure>
3. Click **Sign in**, enter your credentials, and click **Yes, Continue to FlowiseAI**.

    <figure><img src="../../.gitbook/assets/zapier/zap/16.png" alt="" width="563"><figcaption></figcaption></figure>

    <figure><img src="../../.gitbook/assets/zapier/zap/17.png" alt="" width="563"><figcaption></figcaption></figure>
4. Select **Content** from Discord and your Flow ID, then click **Continue**.

    <figure><img src="../../.gitbook/assets/zapier/zap/18.png" alt="" width="563"><figcaption></figcaption></figure>
5. Click **Test action** and wait for the result.

    <figure><img src="../../.gitbook/assets/zapier/zap/19.png" alt="" width="563"><figcaption></figcaption></figure>

### Sending Result Messages

1. Click **+**, then click or search for **Discord**.

    <figure><img src="../../.gitbook/assets/zapier/zap/20.png" alt="" width="563"><figcaption></figcaption></figure>
2. Select the **Send Channel Message** event and click **Continue**.

    <figure><img src="../../.gitbook/assets/zapier/zap/21.png" alt="" width="563"><figcaption></figcaption></figure>
3. Select the signed-in Discord account and click **Continue**.

    <figure><img src="../../.gitbook/assets/zapier/zap/22.png" alt="" width="563"><figcaption></figcaption></figure>
4. Select your desired channel, select **Text**, and select **String Source** (if available) from FlowiseAI for the Message Text, then click **Continue**.

    <figure><img src="../../.gitbook/assets/zapier/zap/23.png" alt="" width="563"><figcaption></figcaption></figure>
5. Click **Test action**.

    <figure><img src="../../.gitbook/assets/zapier/zap/24.png" alt=""><figcaption></figcaption></figure>
6. The message should now appear in your Discord channel. 🎉

    <figure><img src="../../.gitbook/assets/zapier/zap/25.png" alt=""><figcaption></figcaption></figure>
7. Finally, rename your Zap and publish it.

    <figure><img src="../../.gitbook/assets/zapier/zap/26.png" alt=""><figcaption></figcaption></figure>
