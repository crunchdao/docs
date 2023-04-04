---
description: How to submit step-by-step guide
---

# Submissions

## Time to submit

A new `round` starts every week at `Friday at 18:00 UTC` and new tournament data is released. To participate in the round, you must submit your prediction file by the deadline on `Tuesday 9:00 UTC`.

## Before submitting

Before submitting make sure your prediction file includes the right column labels `id`, `Moons`, `target_w`, `target_r`_,_ `target_g`, `target_b` as shown in this example:

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Example of prediction file</p></figcaption></figure>

Be aware that your sumbission file must contain the same `Moons` and the same `ids` in each `moon` than X\_test.

### How to submit

#### From the website

On the [main page](https://tournament.crunchdao.com/), you can click on this button to submit your work:

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

#### From the API - recommended

To upload your submission directly from your pipeline you can use this API endpoint:&#x20;

````python
```python3
r = requests.post(
            f"https://api.tournament.crunchdao.com/v2/submissions",
            params={
                "apiKey": API_KEY, # <- Your API key
            },
            files = {
                "file": ("x", prediction_dataframe.to_csv(index=False).encode('ascii'))
            },
            data = {
                "comment": "" # Write a comment about your model here
            },
        )
```
````

A functional example can be found in the quickstart:

{% embed url="https://colab.research.google.com/drive/1y-014Hz--erBxSzTxgXIdv5Pc3UTqFeK#scrollTo=s27mjZWMmQms" %}
How to submit notebook
{% endembed %}
