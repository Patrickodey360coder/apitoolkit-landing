---
title: Django
ogTitle: Django SDK Guide
date: 2022-03-23
updatedDate: 2024-06-17
menuWeight: 1
---

# Django SDK Guide

To integrate your Django application with APItoolkit, you need to use this SDK to monitor incoming traffic, aggregate the requests, and then send them to APItoolkit's servers. Kindly follow this guide to get started and learn about all the supported features of APItoolkit's **Django SDK**.

```=html
<hr>
```

## Prerequisites

Ensure you have already completed the first three steps of the [onboarding guide](/docs/onboarding/){target="\_blank"}.

## Installation

Kindly run the command below to install the SDK:

```sh
pip install apitoolkit-django
```

## Configuration

First, add the `APITOOLKIT_KEY` environment variable to your `.env` file, like so:

```sh
APITOOLKIT_KEY={ENTER_YOUR_API_KEY_HERE}
```

Next, add the `APITOOLKIT_KEY` to your Django settings (`settings.py`) file, like so:

```python
from pathlib import Path
from dotenv import load_dotenv
import os

load_dotenv()

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent

APITOOLKIT_KEY = os.getenv('APITOOLKIT_KEY')
APITOOLKIT_DEBUG = False
APITOOLKIT_TAGS = ["environment: production", "region: us-east-1"]
APITOOLKIT_SERVICE_VERSION = "v2.0"

# Application definition
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.sessions',
]

...
```

<div class="callout">
  <p><i class="fa-regular fa-lightbulb"></i> <b>Tip</b></p>
  <ol>
  <li>The `{ENTER_YOUR_API_KEY_HERE}` demo string should be replaced with the API key generated from the APItoolkit dashboard.</li>
  <li class="mt-6">The `APITOOLKIT_KEY` variable is required but you can add the following optional variables:</li>
  <ul>
    <li>`APITOOLKIT_DEBUG`: Set to `true` to enable debug mode.</li>
    <li>`APITOOLKIT_TAGS`: A list of defined tags for your services (used for grouping and filtering data on the dashboard).</b></li>
    <li>`APITOOLKIT_SERVICE_VERSION`: A defined string version of your application (used for further debugging on the dashboard).</li>
  </ul>
  </ol>
</div>

Then add the `apitoolkit_django.APIToolkit` middleware into the `settings.py` middleware list, like so:

```python
MIDDLEWARE = [
    'apitoolkit_django.APIToolkit', # Initialize APItoolkit
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    ...,
]
```

## Redacting Sensitive Data

If you have fields that are sensitive and should not be sent to APItoolkit servers, you can mark those fields to be redacted (the fields will never leave your servers).

To mark a field for redacting via this SDK, you need to add some additional environmental fields to the `.env` file with paths to the fields that should be redacted. There are three variables you can provide to configure what gets redacted, namely:

1. `APITOOLKIT_REDACT_HEADERS`: A list of HTTP header keys.
2. `APITOOLKIT_REDACT_REQ_BODY`: A list of JSONPaths from the request body.
3. `APITOOLKIT_REDACT_RES_BODY`: A list of JSONPaths from the response body.

<hr />
JSONPath is a query language used to select and extract data from JSON files. For example, given the following sample user data JSON object:

```JSON
{
  "user": {
    "name": "John Martha",
    "email": "john.martha@example.com",
    "addresses": [
      {
        "street": "123 Main St",
        "city": "Anytown",
        "state": "CA",
        "zip": "12345"
      },
      {
        "street": "123 Main St",
        "city": "Anytown",
        "state": "CA",
        "zip": "12345"
      },
      ...
    ],
    "credit_card": {
      "number": "4111111111111111",
      "expiration": "12/28",
      "cvv": "123"
    }
  },
  ...
}
```

Examples of valid JSONPaths would be:

- `$.user.credit_card` (In this case, APItoolkit will replace the `addresses` field inside the `user` object with the string `[CLIENT_REDACTED]`).
- `$.user.addresses[*].zip` (In this case, APItoolkit will replace the `zip` field in all the objects of the `addresses` list inside the `user` object with the string `[CLIENT_REDACTED]`).

<div class="callout">
  <p><i class="fa-regular fa-lightbulb"></i> <b>Tip</b></p>
  <p>To learn more about JSONPaths, please take a look at the [official docs](https://github.com/json-path/JsonPath/blob/master/README.md){target="_blank"}. You can also use this [JSONPath Evaluator](https://jsonpath.com?utm_source=apitoolkit){target="_blank"} to validate your JSONPaths.</p>
</div>
<hr />

Here's an example of what the `settings.py` file would look like with redacted fields:

```python
APITOOLKIT_REDACT_HEADERS = ["content-type", "Authorization", "HOST"]
APITOOLKIT_REDACT_REQ_BODY = ["$.user.email", "$.user.addresses"]
APITOOLKIT_REDACT_RES_BODY = ["$.users[*].email", "$.users[*].credit_card"]
```

<div class="callout">
  <p><i class="fa-regular fa-circle-info"></i> <b>Note</b></p>
  <ul>
    <li>The `APITOOLKIT_REDACT_HEADERS` variable expects a list of <b>case-insensitive headers as strings</b>.</li>
    <li>The `APITOOLKIT_REDACT_REQ_BODY` and `APITOOLKIT_REDACT_RES_BODY` variables expect a list of <b>JSONPaths as strings</b>.</li>
    <li>The list of items to be redacted will be applied to all endpoint requests and responses on your server.</li>
  </ul>
</div>

## Error Reporting

APItoolkit automatically detects different unhandled errors, API issues, and anomalies but you can report and track specific errors at different parts of your application. This will help you associate more detail and context from your backend with any failing customer request.

To report all uncaught errors that happened during a web request, use the `report_error()` function from the `apitoolkit_django` module, passing in the `request` and `error`, like so:

```python
from django.http import JsonResponse
from apitoolkit_django import observe_request, report_error

def hello_world(request, name):
    try:
        resp = observe_request(request).get(
            "https://jsonplaceholder.typicode.com/todos/2")
        resp.read()
        return JsonResponse({"hello": "world"})
    except Exception as e:
        report_error(request, e)
        return JsonResponse({"Error": "Something went wrong..."})
```

## Monitoring Outgoing Requests

Outgoing requests are external API calls you make from your API. By default, APItoolkit monitors all requests users make from your application and they will all appear in the [API Log Explorer](/docs/dashboard/dashboard-pages/api-log-explorer/){target="\_blank"} page. However, you can separate outgoing requests from others and explore them in the [Outgoing Integrations](/docs/dashboard/dashboard-pages/outgoing-integrations/){target="\_blank"} page, alongside the incoming request that triggered them.

To monitor outgoing HTTP requests from your application, use the `observe_request()` function from the `apitoolkit_django` module, like so:

```python
from django.http import JsonResponse
from apitoolkit_django import observe_request, report_error


def hello_world(request, name):
    resp = observe_request(request).get(
        "https://jsonplaceholder.typicode.com/todos/2")
    resp.read()
    return JsonResponse({"data": resp.read()})
```

<div class="callout">
  <p><i class="fa-regular fa-lightbulb"></i> <b>Tip</b></p>
  <p>The `observe_request()` function wraps an [HTTPX](https://python-httpx.org?utm_source=apitoolkit){target="\_blank"} client and you can use it just like you would normally use HTTPX for any request you need.</p>
</div>

```=html
<hr />
<a href="https://github.com/apitoolkit/apitoolkit-django" target="_blank" rel="noopener noreferrer" class="w-full btn btn-outline link link-hover">
    <i class="fa-brands fa-github"></i>
    Explore the Django SDK
</a>
```
