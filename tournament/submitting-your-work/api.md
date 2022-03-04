---
description: Change some submission properties by using the API.
---

# API

To change some properties of a submission, you need to do a PATCH request to your resource's endpoint.

{% swagger method="patch" path="" baseUrl="/v2/submissions/{id}" summary="Partially update a submission." %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="long" required="true" %}
Submission's ID.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="apiKey" type="string" required="true" %}
User's API Key.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="selected" type="boolean" %}
Update the submission's selected state.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="comment" type="string" %}
Update the submission's comment.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="The update has been applied." %}

{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="You do not have the permission to update this submission." %}

{% endswagger-response %}

{% swagger-response status="404: Not Found" description="The submission hasn't been found." %}

{% endswagger-response %}

{% swagger-response status="423: Locked" description="Too much submission has been selected for this round." %}

{% endswagger-response %}

{% swagger-response status="423: Locked" description="The round is closed and no more modification is allowed." %}

{% endswagger-response %}
{% endswagger %}
