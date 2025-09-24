---
description: How to use the API to interact with the website.
---

# Tournament API

## Endpoints

Most endpoints are documented in the Swagger UI, which is available here:

{% embed url="https://api.hub.crunchdao.com/swagger-ui/index.html" %}

## Python Client

The crunch-cli offer an API client to easily use do action on the platform:

{% embed url="https://github.com/crunchdao/crunch-cli/tree/master/crunch/api" %}

### Usage

The following code export the all the leaderboards from the ADIA Lab Structural Break competition:

{% code title="Python Code" %}
```python
import crunch

client = crunch.api.Client.from_env()

competition = client.competitions.get("structural-break")
round_ = competition.rounds.get(1)
phase = round_.phases.submission

for crunch_ in phase.crunches:
    if not crunch_.published:
        continue

    df = competition.leaderboards.get_default(crunch=crunch_).as_dataframe()
    df.to_csv(f"{crunch_.number}.csv", index=False)
```
{% endcode %}

{% hint style="info" %}
The `Client.from_env()` function will search for the `CRUNCH_API_KEY` environment variable.
{% endhint %}

### Features

* Fluent syntax
* Frequently used routines
* Typing
* Maintained by us and always up-to-date
* Error classes, for easier try-except

## Authentication

There are multiple ways to authenticate:

<table><thead><tr><th width="144.84906005859375">Token Type</th><th>Header</th><th>Query Parameter</th></tr></thead><tbody><tr><td>API Key</td><td><code>Authorization: API-Key &#x3C;token></code> </td><td><code>?apiKey=&#x3C;token></code></td></tr><tr><td>Access Token</td><td><code>Authorization: Bearer &#x3C;token></code> </td><td><code>?accessToken=&#x3C;token></code></td></tr></tbody></table>

You can generate an API-Key in the [API Management section of your account](https://hub.crunchdao.com/account/api).

## Error

Any message that does not return a `2XX` or `3XX` error code is considered an error.

All errors are formatted in the following way:

{% code title="API Error Response" %}
```json
{
    // A unique code, always in UPPER AND SNAKE_CASE
    "code": "SUBMISSION_NOT_FOUND",

    // A message, that often, contains more details
    "message": "submission not found",

    // More properties that provide additional context
    "submissionNumber": 123,
    "projectName": "my-model"
}
```
{% endcode %}
