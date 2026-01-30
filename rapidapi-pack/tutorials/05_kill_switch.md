# Kill Switch (Incident Lockdown)

Instantly lock down your AI systems during a security incident.

## What It Does

When activated:
- All `/v2/scan` requests with **medium or higher** risk are **blocked**
- Your agents stop processing risky prompts immediately
- Normal (low risk) traffic continues

---

## Activate Lockdown

```bash
curl -X POST "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/v2/incident/lockdown" \
  -H "Content-Type: application/json" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com" \
  -d '{"scope": "tenant"}'
```

**Response:**
```json
{
  "ok": true,
  "scope": "tenant",
  "on": true
}
```

---

## Check Status

```bash
curl "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/v2/incident/status" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com"
```

**Response:**
```json
{
  "ok": true,
  "tenant": "your-org",
  "global": false,
  "tenant": true
}
```

---

## Deactivate Lockdown

```bash
curl -X POST "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/v2/incident/unlock" \
  -H "Content-Type: application/json" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com" \
  -d '{"scope": "tenant"}'
```

---

## Use Cases

### 1. Active Attack Response
```python
def on_attack_detected():
    # Immediately lock down
    requests.post(".../v2/incident/lockdown", json={"scope": "tenant"})
    
    # Alert security team
    notify_security_team()
    
    # Log the incident
    log_incident("Lockdown activated due to detected attack")
```

### 2. Scheduled Maintenance
```python
# Lock down before maintenance
lockdown()

# Do maintenance...
update_models()

# Verify everything works
run_tests()

# Unlock
unlock()
```

### 3. Gradual Rollout
```python
# Deploy new model version
deploy_new_model()

# Start in lockdown mode (extra cautious)
lockdown()

# Monitor for issues
if no_issues_after(hours=2):
    unlock()
```

---

## Scopes

| Scope | Effect |
|-------|--------|
| `tenant` | Locks down only your organization |
| `global` | Locks down all tenants (admin only) |
