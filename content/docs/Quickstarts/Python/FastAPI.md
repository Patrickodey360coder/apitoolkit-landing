---
title: FastAPI
date: 2023-09-16
publishdate: 2023-09-16
weight: 20
menu:
  main:
    weight: 20
---

The APIToolkit FastAPI SDK provides an easy-to-use library for integrating FastAPI applications with APIToolkit's monitoring and analytics services. This SDK allows you to collect, analyze, and visualize API metrics in real-time, offering insights into your application's performance, errors, and usage.

## Installation

The Installation section guides you through setting up the APIToolkit FastAPI SDK in your development environment. This includes meeting the prerequisites, installing the package, and verifying the installation.

### Prerequisites

Before proceeding, ensure that your system meets the following requirements:

1. **Python Version**: Make sure Python 3.6 or higher is installed. You can check your Python version by running:

    ```bash
    python --version
    ```
  
    or for Python 3 specifically:

    ```bash
    python3 --version
    ```

2. **Virtual Environment (Optional)**: It's recommended to use a Python virtual environment to isolate the package dependencies. You can create one using:

    ```bash
    python3 -m venv myenv
    ```

    To activate the environment:

    - On macOS and Linux:
    
        ```bash
        source myenv/bin/activate
        ```
  
    - On Windows:
  
        ```bash
        .\myenv\Scripts\Activate
        ```
  
3. **FastAPI and Uvicorn**: If you haven't installed FastAPI and Uvicorn, you can do so with:

    ```bash
    pip install fastapi uvicorn
    ```

### Installation Steps

1. **Install via pip**: Run the following command to install the APIToolkit FastAPI SDK:

    ```bash
    pip install apitoolkit-fastapi
    ```

    This will download and install the latest version of the SDK.

2. **Verify Installation**: To ensure that the SDK was installed correctly, run:

    ```python
    python -c "import apitoolkit_fastapi"
    ```

    If no errors are displayed, the SDK was successfully installed.

### Troubleshooting

1. **Installation Fails**: If you encounter any issues during the installation, try upgrading pip and setuptools:

    ```bash
    pip install --upgrade pip setuptools
    ```
  
    Then try the installation again.

2. **Import Errors**: If you're experiencing import errors after installation, double-check that you're using the Python interpreter from the virtual environment where the SDK was installed.

3. **Dependencies Conflicts**: If there's a conflict with package versions, consider isolating your project using a virtual environment, as mentioned in the prerequisites.



## Usage

The Usage section outlines the steps to effectively incorporate the APIToolkit FastAPI SDK into your application. This includes importing the SDK, initializing it, and adding it to your FastAPI project.

### Importing the SDK

1. **Locate Main File**: Navigate to the main file of your FastAPI application where you've instantiated FastAPI. This is usually `main.py` or `app.py`.

2. **Import Statement**: Add the following line at the top of your file to import the APIToolkit SDK.

    ```python
    from apitoolkit_fastapi import APIToolkit
    ```

    Make sure this line is placed after the FastAPI import to avoid any dependency issues.

### Initialization

1. **Instantiate FastAPI**: If you haven't already, create a FastAPI instance.

    ```python
    from fastapi import FastAPI
    app = FastAPI()
    ```

2. **Create APIToolkit Instance**: Initialize the APIToolkit SDK by creating an instance of the `APIToolkit` class. This requires passing in your FastAPI instance (`app` in this case) and your APIToolkit API key.

    ```python
    apitoolkit_client = APIToolkit(api_key="YOUR_API_KEY", fastapi_app=app)
    ```

3. **Initialize SDK**: Finally, invoke the `init` method to complete the SDK's initialization process.

    ```python
    apitoolkit_client.init()
    ```

### Example Code

Putting it all together, your FastAPI main file would look something like this:

```python
from fastapi import FastAPI
from apitoolkit_fastapi import APIToolkit

app = FastAPI()

# Initialize APIToolkit
apitoolkit_client = APIToolkit(api_key="YOUR_API_KEY", fastapi_app=app)
apitoolkit_client.init()

@app.get("/")
def read_root():
    return {"Hello": "World"}
```

### Test Your Setup

1. **Run your FastAPI application**: If you're using Uvicorn, the command would be:

    ```bash
    uvicorn main:app --reload
    ```

    Replace `main` with the name of your FastAPI main file, if different.

2. **API Calls**: Make a few API calls to verify that data is being sent to the APIToolkit dashboard. You should be able to see metrics and logs related to your API.

---

## Configuration Options

The APIToolkit FastAPI SDK offers a range of configuration options to tailor its behavior according to your application's needs. When you create an instance of the `APIToolkit` class, you can pass in a configuration object to customize its functionality. This section outlines those options and provides examples of how to use them.

### Required Parameters

1. **`api_key`**: This is the API key provided by APIToolkit for authentication. This field is mandatory to ensure secure communication between your FastAPI application and the APIToolkit servers.

    ```python
    api_key="YOUR_API_KEY"
    ```

2. **`fastapi_app`**: This is the FastAPI application instance that the SDK will monitor. This parameter is required for initializing the SDK.

    ```python
    fastapi_app=app
    ```

### Optional Parameters

1. **`redact_headers`**: This is an array of HTTP header field names that you want to redact from the data sent to APIToolkit. This is useful for removing sensitive data like authentication tokens.

    ```python
    redact_headers=["Authorization", "Cookie"]
    ```

2. **`redact_request_body`**: An array of JSONPath expressions specifying which fields should be redacted from the request body. This helps in eliminating sensitive data from request payloads.

    ```python
    redact_request_body=["$.user.password", "$.creditCard.number"]
    ```

3. **`redact_response_body`**: Similar to `redact_request_body`, but for the response body. This ensures that sensitive data is not exposed in API responses.

    ```python
    redact_response_body=["$.api_key", "$.user.social_security_number"]
    ```

### Example Configuration

Here is an example of how to initialize the APIToolkit SDK with all the available configuration options:

```python
from fastapi import FastAPI
from apitoolkit_fastapi import APIToolkit

app = FastAPI()

# Configuration options
config = {
    "api_key": "YOUR_API_KEY",
    "fastapi_app": app,
    "redact_headers": ["Authorization", "Cookie"],
    "redact_request_body": ["$.user.password"],
    "redact_response_body": ["$.api_key"]
}

# Initialize APIToolkit with configuration
apitoolkit_client = APIToolkit(**config)
apitoolkit_client.init()
```

By specifying these configuration options, you can fine-tune how the APIToolkit FastAPI SDK interacts with your application and what data it captures.


## Redacting Sensitive Information

Protecting sensitive information is a critical aspect of any application. The APIToolkit FastAPI SDK offers several options to redact sensitive data from your API requests and responses. This section provides an in-depth guide on how to utilize these features.

### Redacting Headers

HTTP headers often contain sensitive information like authentication tokens or cookies. To redact specific headers:

1. **Identify Headers**: Determine which headers contain sensitive information.
  
2. **Use `redact_headers` Option**: Include the identified headers in the `redact_headers` configuration option when initializing the APIToolkit SDK.

    ```python
    redact_headers = ["Authorization", "X-Secret-Token"]
    ```

    Here is how you integrate it into your existing code:

    ```python
    apitoolkit_client = APIToolkit(
        api_key="YOUR_API_KEY",
        fastapi_app=app,
        redact_headers=["Authorization", "X-Secret-Token"]
    )
    ```

### Redacting Request and Response Fields

Sensitive data often resides in the body of HTTP requests and responses. The SDK allows you to redact these fields using JSONPath expressions.

1. **Identify Sensitive Fields**: Figure out which fields in the request and response bodies are sensitive.

2. **Use `redact_request_body` and `redact_response_body` Options**: Provide JSONPath expressions targeting the fields you want to redact.

    ```python
    redact_request_body = ["$.password", "$.user.credit_card"]
    redact_response_body = ["$.token", "$.user.ssn"]
    ```

    Here's how to include these options during SDK initialization:

    ```python
    apitoolkit_client = APIToolkit(
        api_key="YOUR_API_KEY",
        fastapi_app=app,
        redact_request_body=["$.password", "$.user.credit_card"],
        redact_response_body=["$.token", "$.user.ssn"]
    )
    ```

### Example: Full Redaction Configuration

Combining all the redaction options, your initialization code could look like this:

```python
from fastapi import FastAPI
from apitoolkit_fastapi import APIToolkit

app = FastAPI()

# Initialize APIToolkit with redaction options
apitoolkit_client = APIToolkit(
    api_key="YOUR_API_KEY",
    fastapi_app=app,
    redact_headers=["Authorization", "X-Secret-Token"],
    redact_request_body=["$.password", "$.user.credit_card"],
    redact_response_body=["$.token", "$.user.ssn"]
)
apitoolkit_client.init()
```

By following these guidelines, you can ensure that sensitive data is redacted before it ever leaves your server, thereby enhancing the security of your application.

Congratulations! You've successfully set up FastAPI to send data to APIToolkit. Now you can monitor your API, identify anomalies, and more through your APIToolkit dashboard.