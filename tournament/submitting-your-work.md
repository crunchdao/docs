# ⛏ Submitting your work

If you want to submit your work, please make sure that the order of the rows is kept and that the 3 targets are correctly labeled:

* "target\_r"
* "target\_g"
* "target\_b"

### How to submit

#### From the website

On the [main page](http://tournament.datacrunch.com), there is a button to submit your work.

![](<../.gitbook/assets/image (1).png>)

#### From the API

Uploading by hand your work can be a hassle, especially if you have to download it first from a notebook (like Google Colab). To counter this problem, an API endpoint is available.

{% swagger baseUrl="https://api.tournament.datacrunch.com/v2/submissions" path="" method="post" summary="/v2/submissions" %}
{% swagger-description %}
Submit your work
{% endswagger-description %}

{% swagger-parameter in="query" name="apiKey" type="string" %}
Your API Key
{% endswagger-parameter %}

{% swagger-parameter in="body" name="file" type="string" %}
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

### Submission Limit

The current limit of submission per round is 10.

This value can be incremented by referring some people. Each level will get you one more submission.

| Referred Count | Bonus Submission |
| -------------- | ---------------- |
| 1              | 1                |
| 3              | 1                |
| 5              | 1                |
| 8              | 1                |
| 10             | 1                |

If you are experiencing a problem due to a bug from datacrunch's side, the team sometimes give (round limited) extra submission bonus. This is of course exceptional and only if a problem is preventing you to properly participate to the tournament.

### How and when are the score calculated?

The score are being computed every 5 minutes.

If you try to re-submit in the same crunch, your submission will override the previously uploaded one. This allow you a margin of error but is not a live-saver however, please think before trying to submit your work.

The scores are being computed using the [Spearman Ratio](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html).
