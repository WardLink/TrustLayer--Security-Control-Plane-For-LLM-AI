# Getting Started with TrustLayer AI v2

TrustLayer is an AI safety control plane that protects your LLM applications from prompt injection, policy violations, and behavioral drift.

## Quick Start (2 minutes)

### 1. Subscribe on RapidAPI
Get your API key from RapidAPI after subscribing to TrustLayer AI.

### 2. Test the Health Endpoint
```bash
curl "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/health" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com"
```

### 3. Scan Your First Prompt
```bash
curl -X POST "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/v2/scan" \
  -H "Content-Type: application/json" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com" \
  -d '{"prompt": "What is the weather today?"}'
```

**Response:**
```json
{
  "ok": true,
  "verdict": "low",
  "score": 0.05,
  "blocked": false
}
```

That's it! You're now scanning prompts for injection attacks.

---

## Core Concepts

### Verdicts
- **low** (0.0-0.3): Safe to proceed
- **medium** (0.3-0.7): Review recommended
- **high** (0.7-1.0): Block recommended

### Tier Access
| Tier | Endpoints Available |
|------|---------------------|
| Developer | `/v2/scan`, `/v2/contracts` |
| Startup | + Drift detection, Incident status, Policy read |
| Business | + Kill switch, Policy upload, Audit export |

---

## Next Steps
- [Prompt Injection Scanning](./02_prompt_scanning.md)
- [Contract Testing](./03_contract_testing.md)
- [Drift Detection](./04_drift_detection.md)
- [Kill Switch](./05_kill_switch.md)
