# Kill Switch (Incident Lockdown)

Used during active attacks or abnormal behavior.

## Activate Lockdown
```
POST /v2/incident/lockdown
```

**Request Body**
```json
{
  "scope": "tenant"
}
```

| Scope | Effect |
|-------|--------|
| `tenant` | Locks down only your organization |
| `global` | Locks down all tenants (admin only) |

**Example Request**
```bash
curl -X POST "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/v2/incident/lockdown" \
  -H "Content-Type: application/json" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com" \
  -d '{"scope": "tenant"}'
```

**Response**
```json
{
  "ok": true,
  "scope": "tenant",
  "on": true
}
```

**When lockdown is active:**
- All `/v2/scan` requests with medium+ risk are **blocked**
- Low risk traffic continues normally

---

## Check Status
```
GET /v2/incident/status
```

**Example Request**
```bash
curl "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/v2/incident/status" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com"
```

**Response**
```json
{
  "ok": true,
  "tenant": "rapidapi",
  "global": false,
  "tenant": true
}
```

---

## Deactivate Lockdown
```
POST /v2/incident/unlock
```

**Request Body**
```json
{
  "scope": "tenant"
}
```

**Example Request**
```bash
curl -X POST "https://trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com/v2/incident/unlock" \
  -H "Content-Type: application/json" \
  -H "X-RapidAPI-Key: YOUR_API_KEY" \
  -H "X-RapidAPI-Host: trustlayer-ai-control-plane-for-safe-llms-agents.p.rapidapi.com" \
  -d '{"scope": "tenant"}'
```

**Response**
```json
{
  "ok": true,
  "scope": "tenant",
  "on": false
}
```
