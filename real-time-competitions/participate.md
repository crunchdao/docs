---
description: >-
  To get started and submit your first model, you will need to pass through the
  following steps.
---

# Participate

{% hint style="info" %}
Currently in staging phase, please use [https://hub.crunchdao.io/](https://hub.crunchdao.io/) instead of [https://hub.crunchdao.com](https://hub.crunchdao.com).
{% endhint %}

## Create your Solana Wallet

Before you can participate in CrunchDAO's real-time competitions, you'll need to set up a Solana wallet. We recommend using the Phantom wallet for its ease of use and robust features.

1. **Download Phantom**\
   Visit the [Phantom's "How to create a new wallet" post](https://phantom.com/learn/guides/how-to-create-a-new-wallet) and download the wallet extension for your browser or mobile app.
2. **Install and Setup**\
   Follow the on-screen instructions to install Phantom. During the setup process, you'll create a new wallet, which includes generating a secure recovery phrase.
3. **Secure your Recovery Phrase**\
   Write down your recovery phrase and keep it in a safe place. This phrase is essential for recovering your wallet if you lose access to your device.



{% hint style="info" %}
For testing and development on the Devnet, you'll need to add test SOL tokens to your wallet. Visit the official [Solana Faucet](https://faucet.solana.com/), enter your wallet address, connect to your GitHub account, and request the desired amount of test tokens. These tokens are provided for staging purposes only and have no real-world value.
{% endhint %}

## Accept the Rules (Onchain/Offchain)

Go to a real-time competition you want to join, which is indicated by a spinning clock icon.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

You'll need to formally accept the competition rules. To do so, navigate to the "**Overview**" section.

<figure><img src="../.gitbook/assets/image (146).png" alt=""><figcaption></figcaption></figure>

And click the "**Read the rules**" button. This action will trigger a Solana transaction that records your acceptance on the blockchain. Additionally, your wallet's public key will be stored on our platform for future withdrawals, though you'll have the option to update this information later if necessary.

<figure><img src="../.gitbook/assets/image (147).png" alt=""><figcaption></figcaption></figure>

## Create a Model

Next, it's time to create a model. Go to "**My Models**".

<figure><img src="../.gitbook/assets/image (148).png" alt=""><figcaption></figcaption></figure>

Click on "**+ New Model**", enter the name of your model and click on the "**Create**" button. Once this is done, you're ready to submit on this model.

<figure><img src="../.gitbook/assets/image (149).png" alt=""><figcaption></figcaption></figure>

Depending on the competition, you may have the option to create and submit one or multiple models.

## Submit your Code

To submit your code, go to the "**Submit**" page.

<figure><img src="../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

Select your preferred submission method. [The submission process is the same as for other competitions.](../competitions/participate/#submit)

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## Deploy your Submission

Submission is just as easy. Go to your models page where you'll find a new entry in the "**Submissions**" section. Simply click the "**Deploy**" button and follow Phantom's instructions to record the transaction on the blockchain.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

By default, your deployment will appear under the "**Model Runner**" section as "**On**". You can adjust this status at any time by simply toggling the switch.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Once the deployment is complete, a new entry is added to the "**Deployments**" section where you can monitor and manage your deployed models. From there, you'll also have access to Model Runner logs to track performance and troubleshoot if necessary.

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

## Check your Runner/Builder Logs

The logs are divided into two sections:

* **Builder**: the logs generated while building the image of your model
* **Runner**: the logs of your model running live

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Only the last 100 lines are available.
{% endhint %}
