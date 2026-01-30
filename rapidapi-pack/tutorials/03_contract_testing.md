# Contract Testing

Run multiple safety checks in a single API call.

## What It Tests

| Contract | Detects |
|----------|---------|
| `prompt_injection` | Jailbreaks, instruction overrides |
| `pii_detection` | SSN patterns, personal data |
| `tool_hijack` | Dangerous tool invocations |

---

## Basic Usage

### Request
```bash
curl -X POST "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/v2/contracts" \
  -H "Content-Type: application/json" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com" \
  -d '{
    "text": "My social security number is 123-45-6789"
  }'
```

### Response
```json
{
  "ok": true,
  "passed": false,
  "failed_count": 1,
  "checks": [
    {
      "name": "prompt_injection",
      "verdict": "low",
      "score": 0.05,
      "pass": true
    },
    {
      "name": "pii_detection",
      "verdict": "high",
      "score": 0.85,
      "pass": false
    },
    {
      "name": "tool_hijack",
      "verdict": "low",
      "score": 0.05,
      "pass": true
    }
  ]
}
```

---

## Use Cases

### 1. CI/CD Pipeline
```bash
# In your GitHub Action or CI script
result=$(curl -s -X POST ".../v2/contracts" -d '{"text":"$PROMPT"}')
passed=$(echo $result | jq '.passed')

if [ "$passed" = "false" ]; then
  echo "Safety check failed!"
  exit 1
fi
```

### 2. Pre-flight Check
```python
def safe_to_send(text):
    result = requests.post(".../v2/contracts", json={"text": text}).json()
    return result["passed"]

if safe_to_send(user_input):
    response = llm.chat(user_input)
else:
    response = "I cannot process that request."
```

### 3. Compliance Logging
```python
result = check_contracts(text)
if not result["passed"]:
    log_compliance_violation(
        text=text,
        failed_checks=result["checks"],
        timestamp=datetime.now()
    )
```
