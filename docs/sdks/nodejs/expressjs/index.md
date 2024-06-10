---
title: ExpressJs
ogTitle: ExpressJs SDK Guide
date: 2022-03-23
updatedDate: 2024-06-10
menuWeight: 2
---

# ExpressJs SDK Guide

To integrate your ExpressJs application with APItoolkit, you need to use this SDK to monitor incoming traffic, aggregate the requests, and then send them to APItoolkit's servers. Kindly follow this guide to get started and learn about all the supported features of APItoolkit's **ExpressJs SDK**.

```=html
<hr>
```

## Prerequisites

Ensure you have already completed the first three steps of the [onboarding guide](/docs/onboarding/){target="_blank"}.

## Installation

Kindly run the command below to install the SDK:

```sh
npm install apitoolkit-express

# Or

yarn install apitoolkit-express
```

## Configuration

Next, initialize APItoolkit in your application's entry point (e.g., `index.js`) like so:

<section>
  <div class="tab-buttons">
      <div class="tab-button active" onclick="openTab(event, 'Tab1')">ESM</div>
      <div class="tab-button" onclick="openTab(event, 'Tab2')">CommonJs</div>
  </div>
  <div id="Tab1" class="tab-content active">

```js
import { APIToolkit } from "apitoolkit-express";
import express from "express";
import axios from "axios";

const app = express();
const port = 3000;

const apitoolkitClient = APIToolkit.NewClient({ apiKey: "{ENTER_YOUR_API_KEY_HERE}" });

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(apitoolkitClient.expressMiddleware);

app.get("/", (req, res) => {
  res.json({ hello: "Hello world!" });
});

app.listen(port, () => console.log("App running on port: " + port));
```
  </div>
  <div id="Tab2" class="tab-content">

```js
const express = require("express");
const APIToolkit = require("apitoolkit-express").default;

const app = express();
const port = 3000;

const apitoolkitClient = APIToolkit.NewClient({
  apiKey: "{ENTER_YOUR_API_KEY_HERE}",
});

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(apitoolkitClient.expressMiddleware);

app.get("/", (req, res) => {
  res.json({ hello: "Hello world!" });
});

app.listen(port, () => {
  console.log("App running on port: " + port);
});
```
  </div>
</section>

<div class="callout">
  <p><i class="fa-regular fa-lightbulb"></i> <b>Tip</b></p>
  <p>The `{ENTER_YOUR_API_KEY_HERE}` demo string should be replaced with the API key generated from the APItoolkit dashboard.</p>
</div>

## Redacting Sensitive Data

If you have fields that are sensitive and should not be sent to APItoolkit servers, you can mark those fields to be redacted  (the fields will never leave your servers).

To mark a field for redacting via this SDK, you need to add some additional arguments to the `apitoolkitClient` config object with paths to the fields that should be redacted. There are three arguments you can provide to configure what gets redacted, namely:

1. `redactHeaders`:  A list of HTTP header keys.
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
import express from "express";
import { APIToolkit } from "apitoolkit-express";

const app = express();
const port = 3000;

const apitoolkitClient = await APIToolkit.NewClient({
  apiKey: "{ENTER_YOUR_API_KEY_HERE}",
  redactHeaders: ["Content-Type", "Authorization", "HOST"],
  redactRequestBody: ["$.user.email", "$.user.addresses"], 
  redactResponseBody: ["$.users[*].email", "$.users[*].credit_card"]
});

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(apitoolkitClient.expressMiddleware);

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(port, () => {
  console.log("App running on port " + port);
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

## Handling File Uploads

If you handling file uploads in your application using a framework, you might need to add some extra configuration to ensure that the request object contains all the parsed data, enabling accurate monitoring of file uploads across your application.

Working with file uploads using [multer](https://npmjs.com/package/multer){target="_blank" rel="noopener noreferrer"} for example requires no extra work since the parsed data (`fields` and `files`) are attached to the request object in multipart/form-data requests. However, if you're using [formidable](https://npmjs.com/package/formidable){target="_blank" rel="noopener noreferrer"}, you need to attach the parsed data yourself to ensure accurate monitoring. Here's an example:

```js
import express from "express";
import { APIToolkit } from "apitoolkit-express";
import formidable from "formidable";

const app = express();
const port = 3000;

const client = APIToolkit.NewClient({
  apiKey: "{ENTER_YOUR_API_KEY_HERE}",
});

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(client.expressMiddleware);

app.post("/upload-formidable", (req, res, next) => {
  const form = formidable({});
  form.parse(req, (err, fields, files) => {
    // Attach fields to request body
    req.body = fields;
    // Attach files to request body
    req.files = files;

    res.json({ message: "Uploaded successfully!" });
  });
});

app.listen(port, () => {
  console.log("App running on port " + port);
});
```

## Error Reporting

APItoolkit detects different API issues and anomalies automatically but you can report and track specific errors at different parts of your application. This will help you associate more detail and context from your backend with any failing customer request.

### Report All Errors

To report all uncaught errors that happened during a request, you can use the `errorHandler` middleware immediately after your app's controllers like so:

```js
import { APIToolkit, ReportError } from "apitoolkit-express";
import express from "express";
import axios from "axios";

const app = express();
const port = 3000;

const apitoolkitClient = APIToolkit.NewClient({ apiKey: "{ENTER_YOUR_API_KEY_HERE}" });

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(apitoolkitClient.expressMiddleware);

// All controllers
app.get("/", (req, res) => {});
// Other controllers...

// Add the error handler
app.use(apitoolkitClient.errorHandler);

app.listen(port, () => {
  console.log("App running on port " + port);
});
```

<div class="callout">
  <p><i class="fa-regular fa-lightbulb"></i> <b>Tip</b></p>
  <p>Ensure to add the error handler before any other error middleware and after all controllers.</p>
</div>

### Report Specific Errors

To manually report errors within the context of a web request handler, use the `ReportError()` function like so:

```js
import { APIToolkit, ReportError } from "apitoolkit-express";
import express from "express";
import axios from "axios";

const app = express();
const port = 3000;

const apitoolkitClient = APIToolkit.NewClient({ apiKey: "{ENTER_YOUR_API_KEY_HERE}" });

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(apitoolkitClient.expressMiddleware);

app.get("/", (req, res) => {
  try {
    const response = await observeAxios(axios).get(baseURL + "/ping");
    res.send(response);
  } catch (error) {
    // Report the error to APItoolkit
    ReportError(error);
    res.send("Something went wrong...");
  }
});

app.listen(port, () => {
  console.log("App running on port " + port);
});
```

### Report Specific Errors (Background Job)

If your request is called from a background job for example (outside the web request handler and hence, not wrapped by APItoolkit's middleware), using `ReportError()` directly from `apitoolkit-express` will not be available. Instead, call `ReportError()` from `apitoolkitClient` like so:

```js
import {APIToolkit , ReportError } from "apitoolkit-express";
import express from "express";
import axios from "axios"

const app = express();
const port = 3000;

const apitoolkitClient = APIToolkit.NewClient({apiKey: "{ENTER_YOUR_API_KEY_HERE}"});

app.use(apitoolkitClient.expressMiddleware);

app.get("/", (req, res) => {
  try {
  const response = await observeAxios(axios).get(baseURL + "/ping");
  res.send(response);
} catch (error) {
  // Report the error to APItoolkit
  apitoolkitClient.ReportError(error);
  res.send("Something went wrong...")
}
});

app.listen(port, () => {
  console.log("App running on port " + port);
});
```

## Monitoring Outgoing Requests

Outgoing requests are external API calls you make from your API. By default, APItoolkit monitors all requests users make from your application and they will all appear in the [API Log Explorer](/docs/dashboard/dashboard-pages/api-log-explorer/){target="_blank"} page. However, you can separate outgoing requests from others and explore them in the [Outgoing Integrations](/docs/dashboard/dashboard-pages/outgoing-integrations/){target="_blank"} page, alongside the incoming request that triggered them.

### Monitor All Requests

To enable global monitoring of all axios requests with APItoolkit, import the axios instance into the `apitoolkitClient` configuration options like so:

```js
import APIToolkit from "apitoolkit-express";
import axios from "axios";
import express from "express";

const app = express();
const port = 3000;

const apitoolkitClient = APIToolkit.NewClient({
  apiKey: "{ENTER_YOUR_API_KEY_HERE}",
  monitorAxios: axios,
});

app.use(apitoolkitClient.expressMiddleware);

app.get("/", async (req, res) => {
  // This outgoing axios request will be monitored
  const response = await axios.get(baseURL + "users/123");
  res.send(response.data);
});

app.listen(port, () => {
  console.log("App running on port " + port);
});
```

### Monitor Specific Requests

To monitor a specific axios request within the context of a web request handler, wrap your axios instance with the APIToolkit `observeAxios()` function like so:

```js
import APIToolkit, { observeAxios } from "apitoolkit-express";
import axios from "axios";
import express from "express";

const app = express();
const port = 3000;

const apitoolkitClient = APIToolkit.NewClient({ apiKey: "{ENTER_YOUR_API_KEY_HERE}" });

app.use(apitoolkitClient.expressMiddleware);

const pathWildCard = "/users/{user_id}";
const redactHeadersList = ["Content-Type", "Authorization", "HOST"];
const redactRequestBodyList = ["Content-Type", "Authorization", "HOST"];
const redactResponseBodyList = ["$.users[*].email", "$.users[*].credit_card"];

app.get("/", (req, res) => {
  const response = await observeAxios(
    axios,
    pathWildCard,
    redactHeadersList,
    redactRequestBodyList,
    redactResponseBodyList
  ).get(baseURL + "/users/user1234");
  res.send(response.data);
});

app.listen(port, () => {
  console.log("App running on port " + port);
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

### Monitor Specific Requests (Background Job)

If your outgoing request is called from a background job for example (outside the web request handler and hence, not wrapped by APItoolkit's middleware), using `observeAxios` directly from `apitoolkit-express` will not be available. Instead, call `observeAxios` from `apitoolkitClient` like so:

```js
import axios from "axios";
import { APIToolkit } from "apitoolkit-express";

const apitoolkitClient = APIToolkit.NewClient({
  apiKey: "{ENTER_YOUR_API_KEY_HERE}",
});

const response = await apitoolkitClient
  .observeAxios(axios)
  .get("http://localhost:8080/ping");
console.log(response.data);
```

```=html
<hr />
<a href="https://github.com/apitoolkit/apitoolkit-express" target="_blank" rel="noopener noreferrer" class="w-full btn btn-outline link link-hover">
    <i class="fa-brands fa-github"></i>
    Explore the ExpressJS SDK
</a>
```

```=html
    <script>
        function openTab(event, tabName) {
            let i, tabcontent, tablinks;

            // Hide all tab content
            tabcontent = document.getElementsByClassName("tab-content");
            for (i = 0; i < tabcontent.length; i++) {
                tabcontent[i].style.display = "none";
                tabcontent[i].classList.remove("active");
            }

            // Remove the active class from all tab buttons
            tablinks = document.getElementsByClassName("tab-button");
            for (i = 0; i < tablinks.length; i++) {
                tablinks[i].classList.remove("active");
            }

            // Show the current tab's content and add active class to the button
            document.getElementById(tabName).style.display = "block";
            document.getElementById(tabName).classList.add("active");
            event.currentTarget.classList.add("active");
        }

        // Initialize the first tab to be visible
        document.addEventListener("DOMContentLoaded", function () {
            document.querySelector(".tab-button").click();
        });
    </script>
```
