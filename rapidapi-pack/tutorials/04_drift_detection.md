# Agent Drift Detection

Monitor your AI agents for behavioral changes over time.

## What Is Drift?

Drift occurs when an AI agent's behavior changes unexpectedly:
- Different response style
- New capabilities appearing
- Tool usage patterns changing
- Safety guardrails weakening

---

## How It Works

1. **Set a baseline**: Store your expected "golden" output
2. **Check regularly**: Compare current outputs to baseline
3. **Alert on drift**: Get notified when behavior changes

---

## Step 1: Set Baseline

```bash
curl -X POST "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/v2/drift/baseline" \
  -H "Content-Type: application/json" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com" \
  -d '{
    "suite_id": "support-agent-v1",
    "baseline_output": "I am a helpful support agent. I can answer questions about our products and services. I cannot make purchases or access account details.",
    "baseline_tool_calls": [
      {"name": "search_knowledge_base"},
      {"name": "get_product_info"}
    ]
  }'
```

---

## Step 2: Check for Drift

```bash
curl -X POST "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/v2/drift/check" \
  -H "Content-Type: application/json" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com" \
  -d '{
    "suite_id": "support-agent-v1",
    "current_output": "I can help you transfer money and access your account.",
    "tool_calls": [
      {"name": "transfer_funds"},
      {"name": "get_account_balance"}
    ],
    "threshold": 0.35
  }'
```

### Response
```json
{
  "ok": true,
  "suite_id": "support-agent-v1",
  "drift_score": 0.78,
  "threshold": 0.35,
  "verdict": "drifting",
  "semantic_distance": 0.65,
  "lexical_distance": 0.72,
  "tool_distance": 0.85
}
```

---

## Understanding the Response

| Field | Meaning |
|-------|---------|
| `drift_score` | Combined drift metric (0-1) |
| `verdict` | "stable" or "drifting" |
| `semantic_distance` | How different the meaning is |
| `lexical_distance` | How different the words are |
| `tool_distance` | How different the tool usage is |

---

## Automated Monitoring

```python
def check_agent_health():
    # Run your agent with a test prompt
    agent_output = agent.run("What can you help me with?")
    
    # Check for drift
    result = requests.post(
        ".../v2/drift/check",
        json={
            "suite_id": "my-agent",
            "current_output": agent_output,
            "threshold": 0.35
        }
    ).json()
    
    if result["verdict"] == "drifting":
        alert_team(f"Agent drift detected! Score: {result['drift_score']}")
        
# Run daily
schedule.every().day.at("09:00").do(check_agent_health)
```
