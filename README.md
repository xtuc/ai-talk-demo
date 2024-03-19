# Cloudflare Workers AI demo


## Ask Llama to generate a prompt

```python
import requests
import os

endpoint = "https://api.cloudflare.com/client/v4/accounts/85d0c02b57ed75faf9b18f92d5c01602/ai/run"
url = "{endpoint}/@cf/meta/llama-2-7b-chat-int8".format(endpoint=endpoint)

headers = {
    "Content-Type": "application/json",
    "Authorization": "Bearer {token}".format(token=os.getenv("API_TOKEN"))
}

payload = {
  "messages": [
    {
        "role": "system",
        "content":
            "You are a simple and creative assistant that writes a prompt for a stable diffusion model, in a realistic and HD style."
            "Reply with the prompt only, avoid 'Here is the prompt...', double quotes and 'Prompt: '."
    },
    {
      "role": "user",
      "content": "cute black and gray cat that looks a bit mad at you and is skinny"
    },
  ]
}
response = requests.request("POST", url, json=payload, headers=headers)
response.raise_for_status()

prompt = response.json()["result"]["response"]
print("Prompt: " + prompt)
```

## Generate the image

```py
url = "{endpoint}/@cf/bytedance/stable-diffusion-xl-lightning".format(endpoint=endpoint)
payload = {
    "prompt": prompt
}
response = requests.request("POST", url, json=payload, headers=headers)
response.raise_for_status()

with open("out.png", 'wb') as f:
    f.write(response.content)
```

## Create an AI gateway

https://dash.cloudflare.com/85d0c02b57ed75faf9b18f92d5c01602/ai/ai-gateway

## Change the endpoint to AI gateway

```
endpoint = "https://gateway.ai.cloudflare.com/v1/85d0c02b57ed75faf9b18f92d5c01602/ai-talk-3/workers-ai"
```
