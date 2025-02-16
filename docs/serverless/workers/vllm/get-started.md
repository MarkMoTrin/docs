---
title: Get started
sidebar_position: 2
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

RunPod provides a simple way to run large language models (LLMs) as a Serverless Endpoint.
vLLM Workers are pre-built Docker images that you can configure entirely within the RunPod UI.

You will learn to deploy an OpenAI compatible Endpoint with vLLM inference engine on RunPod.

## Deploy the vLLM Endpoint

One of the advantages of deploying your model with the vLLM Worker is that you can deploy your model with minimal configuration.
You don't need to write the logic to download your image, bundle it, and push it to a Docker registry.
For most models, you just need to pass in the prebuilt vLLM Worker image name and the LLM model name, and the vLLM Worker takes care of the rest.

This step walks you through running the vLLM Worker on a Serverless Endpoint deploying to RunPod.

1. Log in to the [RunPod Serverless console](https://www.runpod.io/console/serverless).
2. Select **+ New Endpoint**.
3. Provide the following:
   1. Endpoint name.
   2. Select a GPU.
   3. Configure the number of Workers.
   4. (optional) Select **FlashBoot**.
   5. Enter the vLLM RunPod Worker image name, which provides the compatible CUDA version:
      - `runpod/worker-vllm:stable-cuda11.8.0`
      - `runpod/worker-vllm:stable-cuda12.1.0`
   6. (optional) Select a [network storage volume](/serverless/endpoints/manage-endpoints#add-a-network-volume).
   7. Configure the environment variables:
      1. `MODEL_NAME`: (required) the large language model.
         1. For example: `openchat/openchat-3.5-0106`.
      2. `HF_TOKEN`: (optional) your Hugging Face API token for private models.
4. Select **Deploy**.

Once the Endpoint initializes, you can send a request to your [Endpoint](/serverless/endpoints/get-started).

:::note

To use CUDA version 12.1.0 or greater, filter for GPUs that support that version of CUDA by selecting the versions under the **Advanced** tab.

For a complete list of available environment variables, see the [vLLM Worker variables](/serverless/workers/vllm/environment-variables).

:::

You've successfully deployed your vLLM Worker as Serverless Endpoint.
You can start making requests to this Endpoint using the compatible OpenAI inputs.

## Send a request

This step walks you through sending a request to your Serverless Endpoint.
The vLLM Worker can use any Hugging Face model and is compatible with OpenAI's API.
If you have the OpenAI library installed, you can continue using it with the vLLM Worker; or see the [OpenAI documentation](https://platform.openai.com/docs/libraries/python-library) for more information.

For this example, use the `openchat/openchat-3.5-0106` model.

### Initialize your project

Choose your programming language and add the following code to your file.

Provide Endpoint ID and API Key as environment variables.

<Tabs>
  <TabItem value="python" label="Python" default>

Create a file called `main.py` in on your machine with the following code:

```python
from openai import OpenAI
import os

# The environment variable 'YOUR_ENDPOINT_ID' could be:
# 'https://api.runpod.ai/v2/your_actual_endpoint_id/openai/v1'
endpoint_id = os.environ.get("RUNPOD_ENDPOINT_ID")

client = OpenAI(
    base_url = f"https://api.runpod.ai/v2/{endpoint_id}/openai/v1",
    api_key=os.environ.get("RUNPOD_API_KEY"),
)

chat_completion = client.chat.completions.create(
    model="openchat/openchat-3.5-0106",
    messages=[{"role": "user", "content": "Reply with: Hello, World!"}]
)

print(chat_completion)
```

</TabItem>
  <TabItem value="node.js" label="Node.js">

Create a file called `main.js` in on your machine with the following code:

```javascript
import OpenAI from "openai";

// The environment variable 'RUNPOD_ENDPOINT_ID' could be:
// 'https://api.runpod.ai/v2/your_actual_endpoint_id/openai/v1'

const openai = new OpenAI({
  baseURL:
    `https://api.runpod.ai/v2/${process.env.RUNPOD_ENDPOINT_ID}/openai/v1`,
  apiKey: process.env.RUNPOD_API_KEY,
});

const chatCompletion = await openai.chat.completions.create({
  model: "openchat/openchat-3.5-0106",
  messages: [{ role: "user", content: "Reply with: Hello, World!" }],
});

console.log(chatCompletion);
```

</TabItem>
  <TabItem value="curl" label="cURL">

Run the following command in your terminal:

```bash
curl https://api.runpod.ai/v2/${YOUR_ENDPOINT_ID}/openai/v1/chat/completions \
    -H "Content-Type: application/json" \
		-H "Authorization: ${RUNPOD_API_KEY}" \
    -d '{
        "model": "openchat/openchat-3.5-0106",
        "messages": [
            {
                "role": "system",
                "content": "You are a helpful assistant."
            },
            {
                "role": "user",
                "content": "Reply with: Hello, World!"
            }
        ]
    }'
```

</TabItem>
</Tabs>

### Run your code

Now run your code from the terminal:

<Tabs>
  <TabItem value="python" label="Python" default>

```bash
python main.py
```

</TabItem>
  <TabItem value="node.js" label="Node.js">

```bash
node main.js
```

</TabItem>
  <TabItem value="curl" label="cURL">

Run the following command in your terminal:

```bash
curl ${RUNPOD_BASE_URL} \
    -H "Content-Type: application/json" \
		-H "Authorization: Bearer ${RUNPOD_API_KEY}" \
    -d '{
        "model": "openchat/openchat-3.5-0106",
        "messages": [
            {
                "role": "system",
                "content": "You are a helpful assistant."
            },
            {
                "role": "user",
                "content": "Reply with: Hello, World!"
            }
        ]
    }'
```

</TabItem>
</Tabs>

The output is as follows:

```json
{
  "choices": [
    {
      "finish_reason": "stop",
      "index": 0,
      "message": {
        "content": "Hello, World!",
        "role": "assistant"
      }
    }
  ],
  "created": 3175963,
  "id": "cmpl-74d7792c92cd4b159292c38bda1286b0",
  "model": "openchat/openchat-3.5-0106",
  "object": "chat.completion",
  "usage": {
    "completion_tokens": 5,
    "prompt_tokens": 39,
    "total_tokens": 44
  }
}
```

You've successfully sent a request to your Serverless Endpoint and received a response.
