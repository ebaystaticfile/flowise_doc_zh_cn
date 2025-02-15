# API

Refer to [API Reference](../api-reference/) for a complete list of public APIs.

## Prediction API

<div data-full-width="false"><figure><img src="../.gitbook/assets/image (16) (1) (1) (1).png" alt=""><figcaption></figcaption></figure></div>

{% swagger src="../.gitbook/assets/swagger (1) (1) (1).yml" path="/prediction/{id}" method="post" %}
[swagger (1) (1) (1).yml](../.gitbook/assets/swagger (1) (1) (1).yml)
{% endswagger %}

### Using Python/TypeScript Libraries

Flowise provides two libraries:

*   [Python](https://pypi.org/project/flowise/): `pip install flowise`
*   [TypeScript](https://www.npmjs.com/package/flowise-sdk): `npm install flowise-sdk`

{% tabs %}
{% tab title="Python SDK" %}
```python
from flowise import Flowise, PredictionData

def test_non_streaming():
    client = Flowise()

    # Test non-streaming prediction
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<chatflow-id>",
            question="What is the capital of France?",
            streaming=False
        )
    )

    # Process and print the response
    for response in completion:
        print("Non-streaming response:", response)

def test_streaming():
    client = Flowise()

    # Test streaming prediction
    completion = client.create_prediction(
        PredictionData(
            chatflowId="<chatflow-id>",
            question="Tell me a joke!",
            streaming=True
        )
    )

    # Process and print each streamed chunk
    print("Streaming response:")
    for chunk in completion:
        print(chunk)


if __name__ == "__main__":
    # Run non-streaming test
    test_non_streaming()

    # Run streaming test
    test_streaming()
```
{% endtab %}

{% tab title="TypeScript SDK" %}
```javascript
import { FlowiseClient } from 'flowise-sdk'

async function test_streaming() {
  const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });

  try {
    // For streaming prediction
    const prediction = await client.createPrediction({
      chatflowId: 'fe1145fa-1b2b-45b7-b2ba-bcc5aaeb5ffd',
      question: 'What is the revenue of Apple?',
      streaming: true,
    });

    for await (const chunk of prediction) {
        console.log(chunk);
    }
    
  } catch (error) {
    console.error('Error:', error);
  }
}

async function test_non_streaming() {
    const client = new FlowiseClient({ baseUrl: 'http://localhost:3000' });
  
    try {
      // For non-streaming prediction
      const prediction = await client.createPrediction({
        chatflowId: 'fe1145fa-1b2b-45b7-b2ba-bcc5aaeb5ffd',
        question: 'What is the revenue of Apple?',
      });
  
      console.log(prediction);
      
    } catch (error) {
      console.error('Error:', error);
    }
}

// Run non-streaming test
test_non_streaming()

// Run streaming test
test_streaming()
```
{% endtab %}
{% endtabs %}

### Configuration Override

Override existing chatflow input configuration using the `overrideConfig` property.

For security reasons, configuration override is disabled by default.  Enable it by navigating to **Chatflow Configuration** -> **Security** tab and selecting the properties to override.

<figure><img src="../.gitbook/assets/image (188).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "sessionId": "123",
        "returnSourceDocuments": True
    }
})
```
{% endtab %}

{% tab title="JavaScript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "sessionId": "123",
        "returnSourceDocuments": True
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### History

Prepend history messages to provide context to the LLM. For example, to make the LLM remember the user's name:

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hey, how are you?",
    "history": [
        {
            "role": "apiMessage",
            "content": "Hello how can I help?"
        },
        {
            "role": "userMessage",
            "content": "Hi my name is Brian"
        },
        {
            "role": "apiMessage",
            "content": "Hi Brian, how can I help?"
        },
    ]
})
```
{% endtab %}

{% tab title="JavaScript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Hey, how are you?",
    "history": [
        {
            "role": "apiMessage",
            "content": "Hello how can I help?"
        },
        {
            "role": "userMessage",
            "content": "Hi my name is Brian"
        },
        {
            "role": "apiMessage",
            "content": "Hi Brian, how can I help?"
        },
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Session Persistence

Use a `sessionId` to maintain conversation state across API calls.  Otherwise, a new session is created each time.

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "sessionId": "123"
    } 
})
```
{% endtab %}

{% tab title="JavaScript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "sessionId": "123"
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Variables

Pass variables to be used by nodes in the flow. See more: [Variables](api.md#variables)

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "vars": {
            "foo": "bar"
        }
    }
})
```
{% endtab %}

{% tab title="JavaScript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Hey, how are you?",
    "overrideConfig": {
        "vars": {
            "foo": "bar"
        }
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Image Uploads

When **Allow Image Upload** is enabled, images can be uploaded from the chat interface.

<div align="left" data-full-width="false"><figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="255"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-02-29 011714.png" alt="" width="290"><figcaption></figcaption></figure></div>

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "question": "Can you describe the image?",
    "uploads": [
        {
            "data": 'data:image/png;base64,iVBORw0KGgdM2uN0', # base64 string or URL
            "type": 'file', # file | url
            "name": 'Flowise.png',
            "mime": 'image/png'
        }
    ]
})
```
{% endtab %}

{% tab title="JavaScript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "question": "Can you describe the image?",
    "uploads": [
        {
            "data": 'data:image/png;base64,iVBORw0KGgdM2uN0', // base64 string or URL
            "type": 'file', // file | url
            "name": 'Flowise.png',
            "mime": 'image/png'
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Speech-to-Text

When **Speech-to-Text** is enabled, users can speak directly into their microphone, and their speech will be transcribed into text.

<div align="left"><figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt="" width="563"><figcaption></figcaption></figure> <figure><img src="../.gitbook/assets/Screenshot 2024-02-29 012538.png" alt="" width="431"><figcaption></figcaption></figure></div>

{% tabs %}
{% tab title="Python API" %}
```python
import requests
API_URL = "http://localhost:3000/api/v1/prediction/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    return response.json()
    
output = query({
    "uploads": [
        {
            "data": 'data:audio/webm;codecs=opus;base64,GkXf', # base64 string
            "type": 'audio',
            "name": 'audio.wav',
            "mime": 'audio/webm'
        }
    ]
})
```
{% endtab %}

{% tab title="JavaScript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/prediction/<chatflowId>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "uploads": [
        {
            "data": 'data:audio/webm;codecs=opus;base64,GkXf', // base64 string
            "type": 'audio',
            "name": 'audio.wav',
            "mime": 'audio/webm'
        }
    ]
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## Vector Upsert API

{% swagger src="../.gitbook/assets/swagger (1) (1) (1).yml" path="/vector/upsert/{id}" method="post" %}
[swagger (1) (1) (1).yml](../.gitbook/assets/swagger (1) (1) (1).yml)
{% endswagger %}

### Document Loaders with File Upload

Several Flowise document loaders support file uploads:

*   [CSV File](../integrations/langchain/document-loaders/csv-file_zh.md)
*   [Docx File](../integrations/langchain/document-loaders/docx-file_zh.md)
*   [JSON File](../integrations/langchain/document-loaders/json-file_zh.md)
*   [JSON Lines File](../integrations/langchain/document-loaders/json-lines-file_zh.md)
*   [PDF File](../integrations/langchain/document-loaders/pdf-file_zh.md)
*   [Text File](../integrations/langchain/document-loaders/text-file_zh.md)
*   [Unstructured File](../integrations/langchain/document-loaders/unstructured-file-loader_zh.md)

<div data-full-width="false"><figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure></div>

For flows using document loaders with file upload functionality, the API uses **form data** instead of JSON to allow file transmission.

{% hint style="info" %}
Ensure the uploaded file type is compatible with the document loader's expected type.  For example, a PDF File Loader requires a `.pdf` file.  For broader compatibility, use the [File Loader](../integrations/langchain/document-loaders/file-loader_zh.md).
{% endhint %}

{% tabs %}
{% tab title="Python API" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowId>"

# Use form data to upload files
form_data = {
    "files": ('state_of_the_union.txt', open('state_of_the_union.txt', 'rb'))
}

body_data = {
    "returnSourceDocuments": True
}

def query(form_data):
    response = requests.post(API_URL, files=form_data, data=body_data)
    print(response)
    return response.json()

output = query(form_data)
print(output)
```
{% endtab %}

{% tab title="JavaScript API" %}
```javascript
// Use FormData to upload files
let formData = new FormData();
formData.append("files", input.files[0]);
formData.append("returnSourceDocuments", true);

async function query(formData) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatflowId>",
        {
            method: "POST",
            body: formData
        }
    );
    const result = await response.json();
    return result;
}

query(formData).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

### Document Loaders without Upload

For document loaders without file upload functionality, the API body is in JSON format, similar to the [Prediction API](api.md#prediction-api).

{% tabs %}
{% tab title="Python API" %}
```python
import requests

API_URL = "http://localhost:3000/api/v1/vector/upsert/<chatflowId>"

def query(payload):
    response = requests.post(API_URL, json=payload)
    print(response)
    return response.json()

output = query({
    "overrideConfig": { # optional
        "returnSourceDocuments": True
    }
})
print(output)
```
{% endtab %}

{% tab title="JavaScript API" %}
```javascript
async function query(data) {
    const response = await fetch(
        "http://localhost:3000/api/v1/vector/upsert/<chatflowId>",
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
        }
    );
    const result = await response.json();
    return result;
}

query({
    "overrideConfig": { // optional
        "returnSourceDocuments": True
    }
}).then((response) => {
    console.log(response);
});
```
{% endtab %}
{% endtabs %}

## Document Upsert/Refresh API

See [Document Stores](document-stores.md#id-10.-api) for API usage details.

{% swagger src="../.gitbook/assets/swagger (2) (1).yml" path="/document-store/upsert/{id}" method="post" %}
[swagger (2) (1).yml](../.gitbook/assets/swagger (2) (1).yml)
{% endswagger %}

{% swagger src="../.gitbook/assets/swagger (2) (1).yml" path="/document-store/refresh/{id}" method="post" %}
[swagger (2) (1).yml](../.gitbook/assets/swagger (2) (1).yml)
{% endswagger %}

## Video Tutorials

These video tutorials demonstrate key Flowise API use cases.

{% embed url="https://youtu.be/9R5zo3IVkqU?si=y1v_aCQLE_70WBnA" %}

{% embed url="https://youtu.be/LhN560DhlzU" %}

{% embed url="https://youtu.be/kOwmPe8aLAA" %}
