# 🚀 Deployment Guide - Two Blueprints Approach

## Overview

Instead of one mega-blueprint, we use **two independent Render blueprints** that talk to each other via HTTP.

| Service | Blueprint | Port | Cost |
|---|---|---|---|
| **N8N** | `render-n8n-blueprint.yaml` | 5678 | ✅ Free |
| **WAHA** | `render-waha-blueprint.yaml` | 3000 | ✅ Free |

Each service gets its own 750 free hours/month quota. Perfect for production.

---

## Step 1️⃣ : Deploy N8N

### Option A: Via Render Dashboard
1. Go to [render.com](https://render.com)
2. Click **New > Blueprint**
3. Connect your GitHub repo
4. Select `render-n8n-blueprint.yaml` as the blueprint file
5. Click **Deploy**

### Option B: Direct Link (if repo is public)
```
https://render.com/deploy?repo=YOUR_GITHUB_REPO_URL&buildFilter=dockerfile
```

**After deploy:**
- You'll get a URL like `https://n8n-xyz.onrender.com`
- Save this URL — you'll need it for WAHA config
- Access N8N at `https://n8n-xyz.onrender.com`

---

## Step 2️⃣ : Deploy WAHA

1. Go to [render.com](https://render.com)
2. Click **New > Blueprint** again
3. Select `render-waha-blueprint.yaml`
4. Click **Deploy**

**After deploy:**
- You'll get `https://waha-xyz.onrender.com`
- **IMPORTANT:** Update the N8N webhook URL to point here

---

## Step 3️⃣ : Connect Them

### In WAHA service (on Render Dashboard):
1. Go to **WAHA service > Environment**
2. Update these variables:

```
WAHA_BASE_URL = https://waha-xyz.onrender.com
WAHA_HOOK_URL = https://n8n-xyz.onrender.com/webhook/waha
```

3. Click **Save** → service will redeploy

### In N8N (via UI):
1. Go to `https://n8n-xyz.onrender.com`
2. Create a webhook that points to WAHA:
   ```
   https://waha-xyz.onrender.com/api/sendMessage
   ```

---

## Step 4️⃣ : Set Secrets (Security)

❌ **DO NOT** leave passwords in plain text YAML!

For each service, override sensitive vars:

### N8N Secrets
```
N8N_ENCRYPTION_KEY  (already auto-generated ✅)
```

### WAHA Secrets
On **WAHA service > Environment**, add as **Secrets** (not regular env):
- `WAHA_API_KEY`
- `WAHA_DASHBOARD_PASSWORD`

---

## 🔗 Testing the Connection

### Test N8N → WAHA
In N8N, create a test workflow:
```
HTTP Request Node
  URL: https://waha-xyz.onrender.com/api/health
  Method: GET
```

Expected response:
```json
{
  "status": "ok",
  "message": "WAHA is running"
}
```

### Test WAHA → N8N
```bash
curl https://waha-xyz.onrender.com/api/status \
  -H "Authorization: Bearer sk_live_51NxA2B3C4D5E6F7G8H9I0J"
```

---

## 📊 Architecture Diagram

```
                    ┌─────────────────┐
                    │  Your Client    │
                    │   (WhatsApp)    │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │   WAHA          │
                    │ (Port 3000)     │
                    │ Messages ↔ WA   │
                    └────────┬────────┘
                             │ Webhook
                    ┌────────▼────────┐
                    │   N8N           │
                    │ (Port 5678)     │
                    │ Automation ✨   │
                    └─────────────────┘
```

---

## 🛑 Troubleshooting

### "Payment information required"
- Make sure you're NOT using database services
- Check Dockerfiles don't request too much RAM
- Each service should be **under 512MB**

### WAHA won't start
- Check `WHATSAPP_START_SESSION` value matches session name
- Verify `/var/lib/waha/storage` directory exists (it does, via Dockerfile)
- Check logs: **WAHA service > Logs** tab

### N8N ↔ WAHA can't talk
- Verify URLs are correct (use full HTTPS URLs)
- Check firewall/CORS: Render services can HTTP to each other
- Use curl to test: `curl https://waha-xyz.onrender.com/api/health`

---

## 💰 Cost Breakdown

| Component | Pricing | Notes |
|---|---|---|
| N8N (web service) | **Free** (750h/mo) | 1 service = ~720h/mo |
| WAHA (web service) | **Free** (750h/mo) | Separate quota ✅ |
| Database | **$0** | Using SQLite locally |
| Storage | **$0** | Ephemeral (ok for sessions) |
| **TOTAL** | **$0/month** | ✨ Production grade |

---

## ✅ Checklist Before Production

- [ ] N8N deployed and accessible
- [ ] WAHA deployed and accessible
- [ ] URLs updated in both services
- [ ] Secrets stored in Render (not in YAML)
- [ ] Test webhook between N8N ↔ WAHA works
- [ ] WAHA QR code scanned (WhatsApp auth complete)
- [ ] N8N workflow created and active
