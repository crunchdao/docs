---
description: How to submit step-by-step guide
---

# ⛏ Submissions

## Time to submit

A new `round` starts every week at `Friday at 18:00 UTC` and new tournament data is released. To participate in the round, you must submit your latest predictions by the deadline on `Monday 9:00 UTC`.

## Before submitting

Before submitting make sure your prediction file includes the right column labels `target_r`_,_ `target_g`, `target_b` as shown in this example:

![Example of prediction file](<../.gitbook/assets/image (29).png>)

### How to submit

#### From the website

On the [main page](http://tournament.datacrunch.com), you can click on this button to submit your work:

![](<../.gitbook/assets/image (1).png>)

#### From the API - recommended

To upload your submission directly from your pipeline you can use this API endpoint:

{% swagger baseUrl="https://api.tournament.datacrunch.com/v2/submissions" path="" method="post" summary="/v2/submissions" %}
{% swagger-description %}
Submit your work
{% endswagger-description %}

{% swagger-parameter in="query" name="apiKey" type="string" required="false" %}
Your API Key
{% endswagger-parameter %}

{% swagger-parameter in="body" name="file" type="string" required="false" %}
File content
{% endswagger-parameter %}

{% swagger-response status="200" description="Submission Submitted" %}
```
:)
```
{% endswagger-response %}

{% swagger-response status="400" description="File content is empty" %}
```
The submitted file content is empty.
```
{% endswagger-response %}

{% swagger-response status="401" description="Email not verified" %}
```
Please verify your email or contact a crew member.
```
{% endswagger-response %}

{% swagger-response status="404" description="Unkown API Key" %}
```
You should check if the provided API Key is valid.
```
{% endswagger-response %}

{% swagger-response status="409" description="Duplicate submission" %}
```
Your work has already been submitted with the same exact results.
If you think that this a false positive, contact a cruncher.

MD5 collision probability: 1/2^128 (source: https://stackoverflow.com/a/288519/7292958)
```
{% endswagger-response %}

{% swagger-response status="423" description="Submissions are close" %}
```
You can only submit during rounds.
Or the server is currently busy crunching the submitted files, please wait some time before retrying.
```
{% endswagger-response %}

{% swagger-response status="429" description="Too many submission" %}
```
You run out of submissions.
```
{% endswagger-response %}

{% swagger-response status="500" description="The server returned an error." %}
```
Ouch!
It seems that we were not expecting this kind of result from the server.
If the probleme persist, contact a crew member.
```
{% endswagger-response %}
{% endswagger %}

Check the example Notebooks to see how to call the API:

{% embed url="https://github.com/datacrunch-com/datacrunch-notebooks" %}
DataCrunch Github repository
{% endembed %}

## Submission Limit

The Tournament is limited to 10 submissions per round.

This value can be incremented by referring some people. Each level will get you one additional submission.

| Referred Count | Bonus Submission |
| -------------- | ---------------- |
| 1              | 1                |
| 3              | 1                |
| 5              | 1                |
| 8              | 1                |
| 10             | 1                |

Before the end of the round you will have to select your best submission. If not we will automatically consider the submission with the best score on the test set.

### Scoring

##

Your score is calculated based on the Mean [Spearman Ratio](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html) correlation between your predictions and the targets.



_**Need more inputs from Jean on scoring functions/method**_
