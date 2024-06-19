---
title: AdonisJs
ogTitle: AdonisJs SDK Guide
date: 2022-03-23
updatedDate: 2024-06-10
menuWeight: 1
---

# AdonisJs SDK Guide

To integrate your AdonisJs application with APItoolkit, you need to use this SDK to monitor incoming traffic, aggregate the requests, and then send them to APItoolkit's servers. Kindly follow this guide to get started and learn about all the supported features of APItoolkit's **AdonisJs SDK**.

```=html
<hr>
```

## Prerequisites

Ensure you have already completed the first three steps of the [onboarding guide](/docs/onboarding/){target="\_blank"}.

## Installation

Kindly run the command below to install the SDK:

```sh
npm install apitoolkit-adonis@latest

# Or

yarn install apitoolkit-adonis@latest
```

<div class="callout">
  <p><i class="fa-regular fa-lightbulb"></i> <b>Tip</b></p>
  <p>If you're using Adonis v5, you will need to instead install `v2.2.1` of the APItoolkit Adonis SDK (i.e., `apitoolkit-adonis@2.2.1`).</p>
</div>

## Configuration

First, run the command below to configure the SDK using ace:

```bash
node ace configure apitoolkit-adonis
```

Then, register the middleware, like so:

<section class="tab-group" data-tab-group="group1">
  <button class="tab-button" data-tab="tab1">Adonis v6 (latest)</button>
  <button class="tab-button" data-tab="tab2">Adonis v5</button>
  <div id="tab1" class="tab-content">
    Add the `apitoolkit-adonis` client to your global middleware list in the `start/kernel.js|ts` file, like so:
        
```js
import server from '@adonisjs/core/services/server'
import APIToolkit from 'apitoolkit-adonis'

const client = new APIToolkit()

server.use([
() => import('#middleware/container_bindings_middleware'),
() => import('#middleware/force_json_response_middleware'),
() => import('@adonisjs/cors/cors_middleware'),
() => client.middleware(),
])
```

  Then, create an `apitoolkit.js|ts` file in the `/conf` directory and export the `defineConfig` object with some properties, like so:

```js
import { defineConfig } from "apitoolkit-adonis";

export default defineConfig({
  apiKey: "{ENTER_YOUR_API_KEY_HERE}",
  debug: false,
  tags: ["environment: production", "region: us-east-1"],
  serviceVersion: "v2.0",
});
```

  </div>
  <div id="tab2" class="tab-content">
    Add `@ioc:APIToolkit` to your global middleware list in the `start/kernel.js|ts` file, like so:
          
```js
Server.middleware.register([
  () => import('@ioc:Adonis/Core/BodyParser'),
  () => import('@ioc:APIToolkit'),
])
```

    Then, create an `apitoolkit.js|ts` file in the `/conf` directory and export an `apitoolkitConfig` object with some properties, like so:

```js
export const apitoolkitConfig = {
  apiKey: "{ENTER_YOUR_API_KEY_HERE}",
  debug: false,
  tags: ["environment: production", "region: us-east-1"],
  serviceVersion: "v2.0",
};
```

  </div>
</section>

<div class="callout">
  <p><i class="fa-regular fa-lightbulb"></i> <b>Tip</b></p>
  <ol>
   <li>The `{ENTER_YOUR_API_KEY_HERE}` demo string should be replaced with the API key generated from the APItoolkit dashboard.</li>
    <li class="mt-6">The configuration accepts a required `apiKey` value and the following optional fields:</li>
      <ul>
        <li>`debug`: Set to `true` to enable debug mode.</li>
        <li>`tags`: A list of defined tags for your services (used for grouping and filtering data on the dashboard).</b></li>
        <li>`serviceVersion`: A defined string version of your application (used for further debugging on the dashboard).</li>
        <li>`disable`: Set to `true` to disable the SDK (data monitoring will be paused).</li>
      </ul>
  </ol>
</div>

## Redacting Sensitive Data

If you have fields that are sensitive and should not be sent to APItoolkit servers, you can mark those fields to be redacted (the fields will never leave your servers).

To mark a field for redacting via this SDK, you need to add some additional arguments to the configuration object in the `conf/apitoolkit.js|ts` file with paths to the fields that should be redacted. There are three arguments you can provide to configure what gets redacted, namely:

1. `redactHeaders`: A list of HTTP header keys.
2. `redactRequestBody`: A list of JSONPaths from the request body.
3. `redactResponseBody`: A list of JSONPaths from the response body.

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

Here's an example of what the configuration would look like with redacted fields:

```js
export default defineConfig({
  apiKey: "{ENTER_YOUR_API_KEY_HERE}",
  debug: false,

  redactHeaders: ["content-type", "Authorization", "HOST"],
  redactRequestBody: ["$.user.email", "$.user.addresses"],
  redactResponseBody: ["$.users[*].email", "$.users[*].credit_card"],
});
```

<div class="callout">
  <p><i class="fa-regular fa-circle-info"></i> <b>Note</b></p>
  <ul>
    <li>The `redactHeaders` config field expects a list of <b>case-insensitive headers as strings</b>.</li>
    <li>The `redactRequestBody` and `redactResponseBody` config fields expect a list of <b>JSONPaths as strings</b>.</li>
    <li>The list of items to be redacted will be applied to all endpoint requests and responses on your server.</li>
  </ul>
</div>

## Error Reporting

APItoolkit automatically detects different unhandled errors, API issues, and anomalies but you can report and track specific errors at different parts of your application. This will help you associate more detail and context from your backend with any failing customer request.

To report errors, you need to first enable [asyncLocalStorage](https://docs.adonisjs.com/guides/concepts/async-local-storage){target="\_blank" rel="noopener noreferrer"} in your AdonisJS project by setting `useAsyncLocalStorage` to `true` in your `config/app.js|ts` file, like so:

<section class="tab-group" data-tab-group="group2">
  <button class="tab-button" data-tab="tab3">Adonis v6 (latest)</button>
  <button class="tab-button" data-tab="tab4">Adonis v5</button>
  <div id="tab3" class="tab-content">

```js
export const http = defineConfig({
  useAsyncLocalStorage: true,
  // Other configs...
});
```

  </div>
  <div id="tab4" class="tab-content">

```js
export const http: ServerConfig = {
  useAsyncLocalStorage: true
  // Other configs...
}
```

  </div>
</section>

<section class="tab-group" data-tab-group="group3">
  <button class="tab-button" data-tab="tab1">Report All Errors</button>
  <button class="tab-button" data-tab="tab2">Report Specific Errors</button>
  <div id="tab1" class="tab-content">
    Then, use the `reportError()` function in your application's exception handler, passing in the `error` argument, to report all uncaught errors and service exceptions that happened during a request, like so:

```js
import { HttpContext, ExceptionHandler } from "@adonisjs/core/http";
import { reportError } from "apitoolkit-adonis";

export default class HttpExceptionHandler extends ExceptionHandler {
  async handle(error: unknown, ctx: HttpContext) {
    return super.handle(error, ctx);
  }

  async report(error: unknown, ctx: HttpContext) {
    // Automatically report all uncaught errors to APItoolkit
    reportError(error);
    return super.report(error, ctx);
  }
}
```

  </div>
  <div id="tab2" class="tab-content">
    Then, use the `reportError()` function, passing in the `error` argument, to manually report errors within the context of a web request handler, like so:

```js
import router from "@adonisjs/core/services/router";
import { reportError } from "apitoolkit-adonis";

router.get("/observer", async () => {
  try {
    throw "Error occurred!";
  } catch (error) {
    // Report the error to APItoolkit
    reportError(error);
  }
  return { hello: "world" };
});
```

  </div>
</section>

## Monitoring Outgoing Requests

Outgoing requests are external API calls you make from your API. By default, APItoolkit monitors all requests users make from your application and they will all appear in the [API Log Explorer](/docs/dashboard/dashboard-pages/api-log-explorer/){target="\_blank"} page. However, you can separate outgoing requests from others and explore them in the [Outgoing Integrations](/docs/dashboard/dashboard-pages/outgoing-integrations/){target="\_blank"} page, alongside the incoming request that triggered them.

To monitor outgoing axios-based HTTP requests from your application, first, enable [asyncLocalStorage](https://docs.adonisjs.com/guides/concepts/async-local-storage){target="\_blank" rel="noopener noreferrer"} in your AdonisJS project by setting `useAsyncLocalStorage` to `true` in your `config/app.js|ts` file, like so:

<section class="tab-group" data-tab-group="group2">
  <button class="tab-button" data-tab="tab3">Adonis v6 (latest)</button>
  <button class="tab-button" data-tab="tab4">Adonis v5</button>
  <div id="tab3" class="tab-content">

```js
export const http = defineConfig({
  useAsyncLocalStorage: true,
  // Other configs...
});
```

  </div>
  <div id="tab4" class="tab-content">

```js
export const http: ServerConfig = {
  useAsyncLocalStorage: true
  // Other configs...
}
```

  </div>
</section>

To monitor all axios request gobally if you're using adonis V6, add `monitorAxios` to the apitoolkit config in the `config/apitoolkit.js|ts` file, like so:

```ts
import { defineConfig } from "apitoolkit-adonis";
import axios from "axios";
export default defineConfig({
  apiKey: "{ENTER_YOUR_API_KEY_HERE}",
  monitorAxios: axios,
});
```

To monitor specific requests (both Adonis v6 and v5) wrap your axios instance with the `observeAxios()` function, like so:

```js
import { observeAxios } from "apitoolkit-adonis";
import axios from "axios";

const pathWildCard = "/users/{user_id}";
const redactHeadersList = ["Content-Type", "Authorization", "HOST"];
const redactRequestBodyList = ["Content-Type", "Authorization", "HOST"];
const redactResponseBodyList = ["$.users[*].email", "$.users[*].credit_card"];

Route.get("/observer", async () => {
  const response = await observeAxios(
    axios,
    pathWildCard,
    redactHeadersList,
    redactRequestBodyList,
    redactResponseBodyList
  ).get(baseURL + "/users/user1234");
  return { hello: "hello world" };
});
```

<div class="callout">
  <p><i class="fa-regular fa-lightbulb"></i> <b>Tip</b></p>
  <p class="mt-6">The `observeAxios` function accepts a required `axios` instance and the following optional fields:</p>
  <ul>
    <li>`pathWildCard`: The `url_path` for URLs with path parameters.</li>
    <li>`redactHeaders`: A string list of headers to redact.</b></li>
    <li>`redactResponseBody`: A string list of JSONPaths to redact from the response body.</li>
    <li>`redactRequestBody`: A string list of JSONPaths to redact from the request body.</li>
  </ul>
</div>

```=html
<hr />
<a href="https://github.com/apitoolkit/apitoolkit-adonis" target="_blank" rel="noopener noreferrer" class="w-full btn btn-outline link link-hover">
    <i class="fa-brands fa-github"></i>
    Explore the AdonisJS SDK
</a>
```
