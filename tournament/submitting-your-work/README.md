---
description: How to submit step-by-step guide
---

# Submissions

## Time to submit

A new `round` starts every week at `Friday at 18:00 UTC` and new tournament data is released. To participate in the round, you must submit your prediction file by the deadline on `Tuesday 9:00 UTC`.

## Before submitting

Before submitting make sure your prediction file includes the right column labels `target_r`_,_ `target_g`, `target_b` as shown in this example:

![Example of prediction file](<../../.gitbook/assets/image (29) (1).png>)

Be also aware that the number of rows in your submission file must match the number of rows in X\_test.

Your prediction must also match the number of lines you have in X\_test.

### How to submit

#### From the website

On the [main page](https://tournament.crunchdao.com/), you can click on this button to submit your work:

![](<../../.gitbook/assets/image (1) (1).png>)

#### From the API - recommended

To upload your submission directly from your pipeline you can use this API endpoint:

A functional example can be found in the quickstart:

{% embed url="https://colab.research.google.com/drive/1ZXARI_CQMbCWs-C_aXEtWPIDZVW61KJ4?usp=sharing" %}
How to submit notebook
{% endembed %}

