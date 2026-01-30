# Agent Drift Detection (v2)

Detect when agent behavior changes unexpectedly over time.

## Step 1: Create a Baseline
```
POST /v2/drift/baseline
```

**Request Body**
```json
{
  "suite_id": "support-agent-v1",
  "baseline_output": "I am a helpful support agent. I answer questions and never perform financial transactions.",
  "baseline_tool_calls": [
    {"name": "search_kb"},
    {"name": "summarize"}
  ]
}
```

**Example Request**
```bash
curl -X POST "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/v2/drift/baseline" \
  -H "Content-Type: application/json" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com" \
  -d '{
    "suite_id": "support-agent-v1",
    "baseline_output": "I am a helpful support agent. I answer questions and never perform financial transactions.",
    "baseline_tool_calls": [
      {"name": "search_kb"},
      {"name": "summarize"}
    ]
  }'
```

**Response**
```json
{
  "ok": true,
  "suite_id": "support-agent-v1",
  "stored": true
}
```

---

## Step 2: Check for Drift
```
POST /v2/drift/check
```

**Request Body**
```json
{
  "suite_id": "support-agent-v1",
  "current_output": "I can transfer money and access your bank account.",
  "tool_calls": [
    {"name": "transfer_funds"}
  ],
  "threshold": 0.35
}
```

**Example Request**
```bash
curl -X POST "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/v2/drift/check" \
  -H "Content-Type: application/json" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com" \
  -d '{
    "suite_id": "support-agent-v1",
    "current_output": "I can transfer money and access your bank account.",
    "tool_calls": [
      {"name": "transfer_funds"}
    ],
    "threshold": 0.35
  }'
```

**Response**
```json
{
  "ok": true,
  "suite_id": "support-agent-v1",
  "tenant": "rapidapi",
  "drift_score": 0.78,
  "threshold": 0.35,
  "verdict": "drifting",
  "semantic_distance": 0.65,
  "lexical_distance": 0.72,
  "tool_distance": 0.85,
  "ts": 1706360400000
}
```

## View Drift Events
```
GET /v2/drift/events?suite_id=support-agent-v1&limit=20
```

**Response**
```json
{
  "ok": true,
  "suite_id": "support-agent-v1",
  "events": [
    {
      "drift_score": 0.78,
      "verdict": "drifting",
      "ts": 1706360400000
    }
  ]
}
```
