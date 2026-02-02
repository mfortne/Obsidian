
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

### Curl
```
curl https://api.openai.com/v1/responses \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer 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" \
  -d '{
    "model": "gpt-5-nano",
    "input": "write a haiku about ai",
    "store": true
  }'
```

### Python
```
pip install openai
```
```
from openai import OpenAI

client = OpenAI(
  api_key="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"
)

response = client.responses.create(
  model="gpt-5-nano",
  input="write a haiku about ai",
  store=True,
)

print(response.output_text);
```