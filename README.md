# Kabowd N8N + WAHA Automation

Production-grade automation platform combining **N8N** (workflow orchestration) and **WAHA** (WhatsApp HTTP API) on Render's free tier.

## 📁 Project Structure

```
project-root/
├── render-n8n-blueprint.yaml        # Blueprint 1: N8N service
├── render-waha-blueprint.yaml       # Blueprint 2: WAHA service
├── DEPLOYMENT.md                   # Step-by-step deployment guide ⭐
├── n8n/
│   └── Dockerfile                  # N8N container config
└── waha/
    └── Dockerfile                  # WAHA container config
```

## 🚀 Quick Start

### Prerequisites
- GitHub account
- Render account (free tier)
- 15 minutes

### Deploy in 3 steps

1. **Push this repo to GitHub**
   ```bash
   git init
   git add .
   git commit -m "Initial deployment"
   git push origin main
   ```

2. **Deploy N8N**
   - Go to [render.com/deploy](https://render.com)
   - Select `render-n8n-blueprint.yaml`
   - Click **Deploy**
   - Wait ~3 min, note the URL: `https://n8n-xyz.onrender.com`

3. **Deploy WAHA**
   - Repeat step 2 with `render-waha-blueprint.yaml`
   - Note the URL: `https://waha-xyz.onrender.com`

👉 **Full guide with troubleshooting:** See [`DEPLOYMENT.md`](./DEPLOYMENT.md)

---

## 🏗️ Architecture

```
WhatsApp Messages
       ↓
   [WAHA] ← receives & sends messages
       ↓ (webhook)
    [N8N] ← processes, logs, forwards
       ↓
   Your Workflows
```

- **WAHA**: WhatsApp gateway (free tier: 512MB)
- **N8N**: Workflow engine (free tier: 512MB)
- **Database**: SQLite (local, no external DB costs ✅)
- **Cost**: $0/month

---

## 🔐 Security

### Secrets Management
Never commit sensitive values. On Render Dashboard:

1. Go to **Service > Environment**
2. Click **Add Secret Variable**
3. Override these:
   - `WAHA_API_KEY`
   - `WAHA_DASHBOARD_PASSWORD`
   - `N8N_ENCRYPTION_KEY` (auto-generated ✅)

### Environment Variables
- `.yaml` files use defaults (for non-sensitive configs)
- **Always override** in Render dashboard for production

---

## 📊 Free Tier Limits

| Resource | Limit | Usage |
|---|---|---|
| Compute hours | 750/month | 2 services × 360h = 720h ✅ |
| Memory | 512MB per service | Both services fit |
| Storage | Ephemeral | Sessions lost on redeploy (planned: add paid disk) |
| Bandwidth | Unlimited | ✅ |

**Total Cost: $0/month** (until you add optional paid features)

---

## 🛠️ Configuration

### N8N Environment Variables
See `render-n8n-blueprint.yaml`:
- `N8N_HOST`: Public URL
- `N8N_PORT`: 5678
- `DB_TYPE`: sqlite (free!)
- `WEBHOOK_URL`: Where external services callback

### WAHA Environment Variables
See `render-waha-blueprint.yaml`:
- `WAHA_BASE_URL`: Public URL
- `WHATSAPP_DEFAULT_ENGINE`: NOWEB (lightweight)
- `WAHA_HOOK_URL`: Where to send webhooks (→ N8N)
- `WAHA_DASHBOARD_*`: Admin panel credentials

---

## 🔌 Integration Example

### Send WhatsApp message via N8N

In N8N workflow:
```
HTTP Request Node
├─ Method: POST
├─ URL: https://waha-xyz.onrender.com/api/sendMessage
├─ Auth: Bearer sk_live_51NxA2B3C4D5E6F7G8H9I0J
└─ Body:
   {
     "chatId": "1234567890@c.us",
     "text": "Hello from N8N!"
   }
```

### Receive WhatsApp messages in N8N

WAHA sends webhooks to:
```
https://n8n-xyz.onrender.com/webhook/waha
```

Configure in N8N:
```
Webhook Trigger Node
├─ Method: POST
├─ Path: /webhook/waha
└─ Process incoming messages...
```

---

## 📝 Logs & Monitoring

### N8N Logs
- Render Dashboard → **n8n service > Logs**
- Or direct via: `https://n8n-xyz.onrender.com/logs`

### WAHA Logs
- Render Dashboard → **waha service > Logs**
- Format: JSON (searchable)
- Log level: `info` (adjustable via env)

---

## 🆘 Support & Troubleshooting

See [`DEPLOYMENT.md`](./DEPLOYMENT.md) for:
- Common errors & fixes
- Architecture diagrams
- Testing procedures
- Cost breakdown

---

## 📜 License

MIT (Kabowd Meetup 2026)

---

## 🎯 Next Steps

1. ✅ Push to GitHub
2. ✅ Deploy N8N blueprint
3. ✅ Deploy WAHA blueprint
4. ✅ Scan WhatsApp QR code in WAHA
5. ✅ Create N8N workflow
6. ✅ Test message flow
7. 🚀 Go live!

Questions? Check [`DEPLOYMENT.md`](./DEPLOYMENT.md)
