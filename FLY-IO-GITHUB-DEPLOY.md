# 🚀 Fly.io + GitHub Automated Deployment

## ✨ Architecture

```
Your GitHub Repo
      ↓
  Push code
      ↓
Fly.io Dashboard
      ↓
Auto-deploy N8N
Auto-deploy WAHA
      ↓
Live at:
- https://kabowd-n8n.fly.dev
- https://kabowd-waha.fly.dev
```

**ZERO command line needed!** All via web dashboard.

---

## 📋 Prerequisites

✅ Fly.io account created (you already have it!)
✅ GitHub account
✅ Code pushed to GitHub

---

## Step 1️⃣ : Push Code to GitHub

### Create repository
1. Go to https://github.com/new
2. Name: `kabowd-automation`
3. Description: "N8N + WAHA WhatsApp Automation"
4. Public or Private (your choice)
5. Click **Create Repository**

### Push your code
```bash
# In your project directory
git init
git add .
git commit -m "Initial commit: N8N + WAHA for Fly.io"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/kabowd-automation.git
git push -u origin main
```

**Your repo is now on GitHub!** ✅

---

## Step 2️⃣ : Connect GitHub to Fly.io

1. Go to **https://fly.io/dashboard**
2. Click **"New App"** (top right)
3. Choose **"Create New App"**

---

## Step 3️⃣ : Deploy N8N

### In Fly.io Dashboard:

1. Click **"Create New App"**

2. **Basic Settings:**
   ```
   App Name: kabowd-n8n
   Organization: (your org)
   ```

3. **Deploy:**
   - Choose **"Deploy from GitHub"**
   - Authorize GitHub
   - Select repo: `YOUR_USERNAME/kabowd-automation`
   - Branch: `main`

4. **Deployment Settings:**
   - Dockerfile: `./n8n/Dockerfile`
   - Build command: (leave empty)

5. **Region:** 
   - Choose `iad` (US - Virginia) or `cdg` (Paris - closer to Africa)

6. **Environment Variables:**
   Click **"Set Environment Variables"** and add:
   ```
   N8N_HOST=kabowd-n8n.fly.dev
   N8N_PORT=5678
   N8N_PROTOCOL=https
   WEBHOOK_URL=https://kabowd-n8n.fly.dev/
   GENERIC_TIMEZONE=Africa/Lome
   DB_TYPE=sqlite
   DB_SQLITE_FILENAME=/data/n8n.sqlite
   N8N_ENCRYPTION_KEY=[generate random string]
   ```

7. Click **"Deploy N8N"** → Wait 2-3 min

**Result:** 
```
✅ N8N deployed at https://kabowd-n8n.fly.dev
```

---

## Step 4️⃣ : Deploy WAHA

Repeat Step 3 but:

1. App Name: `kabowd-waha`
2. Dockerfile: `./waha/Dockerfile`
3. **Environment Variables:**
   ```
   PORT=3000
   WAHA_BASE_URL=https://kabowd-waha.fly.dev
   WAHA_HOOK_URL=https://kabowd-n8n.fly.dev/webhook/waha
   WHATSAPP_DEFAULT_ENGINE=NOWEB
   WAHA_SESSION_NAME=default
   WHATSAPP_START_SESSION=session_production_01
   WAHA_RESTART_ALL_SESSIONS=true
   WAHA_BROWSER_HEADLESS=true
   WAHA_PRINT_QR=true
   WAHA_HOOK_EVENTS=message,message.reaction,presence.update,chat.update
   WAHA_MEDIA_STORAGE=local
   WAHA_LOCAL_STORE_BASE_DIR=/var/lib/waha/storage
   WHATSAPP_FILES_FOLDER=/app/whatsapp_files
   WAHA_LOG_FORMAT=json
   WAHA_LOG_LEVEL=info
   WAHA_DASHBOARD_ENABLED=true
   WAHA_DASHBOARD_USERNAME=admin_waha
   WAHA_DASHBOARD_PASSWORD=SuperSecurePass2026!
   WAHA_API_KEY=sk_live_change_me_in_dashboard
   ```

4. Click **"Deploy WAHA"** → Wait 2-3 min

**Result:**
```
✅ WAHA deployed at https://kabowd-waha.fly.dev
```

---

## Step 5️⃣ : Set Secrets (Better Security)

### For N8N:
1. Go to **Fly.io Dashboard → kabowd-n8n app**
2. Click **"Settings"** → **"Secrets"**
3. Add:
   ```
   N8N_ENCRYPTION_KEY = [strong random string]
   ```

### For WAHA:
1. Go to **Fly.io Dashboard → kabowd-waha app**
2. Click **"Settings"** → **"Secrets"**
3. Add:
   ```
   WAHA_DASHBOARD_PASSWORD = YourSecurePassword123!
   WAHA_API_KEY = sk_live_your_key_here
   ```

---

## Step 6️⃣ : Access Your Services

### N8N Workflow Engine
```
https://kabowd-n8n.fly.dev
```

### WAHA Dashboard
```
https://kabowd-waha.fly.dev/api/dashboard
Username: admin_waha
Password: (from your secrets)
```

---

## Step 7️⃣ : Get WhatsApp QR Code

1. Go to **Fly.io Dashboard → kabowd-waha → Logs**
2. Scroll for `QR CODE:` message
3. Or use Fly CLI if you have it:
   ```bash
   flyctl logs -a kabowd-waha | grep -i qr
   ```

4. Scan the QR code with WhatsApp

---

## Step 8️⃣ : Test Connection

In N8N, create workflow:

**HTTP Request Node:**
```
Method: GET
URL: https://kabowd-waha.fly.dev/api/status
Auth: Bearer [your WAHA_API_KEY]
```

**Expected Response:**
```json
{
  "status": "ok",
  "session": "default"
}
```

---

## 🔄 Auto-Deploy on GitHub Push

Now every time you push to GitHub, Fly.io **automatically redeploys**:

```bash
# Make changes locally
git add .
git commit -m "Update workflow"
git push origin main

# Fly.io automatically detects push and redeploys! ✅
# Check: Fly.io Dashboard → Deployments tab
```

---

## 🛠️ Managing Apps via Dashboard

### View Logs
```
Dashboard → App → Logs tab
```

### Restart App
```
Dashboard → App → Settings → Restart
```

### Update Environment Variables
```
Dashboard → App → Settings → Variables
(Change and save - auto-redeploys)
```

### Check Metrics
```
Dashboard → App → Metrics tab
(Memory, CPU, requests, etc.)
```

---

## 💾 GitHub Repo Structure

```
kabowd-automation/
├── fly-n8n.toml              (Fly.io reads this)
├── fly-waha.toml             (Fly.io reads this)
├── docker-compose.yaml
├── .gitignore
├── README.md
├── n8n/
│   └── Dockerfile
└── waha/
    └── Dockerfile
```

---

## 📋 Complete GitHub + Fly.io Setup Checklist

- [ ] Code pushed to GitHub
- [ ] Fly.io account connected to GitHub (authorized)
- [ ] N8N app created & deployed
- [ ] WAHA app created & deployed
- [ ] Environment variables set (both apps)
- [ ] Secrets configured (passwords, API keys)
- [ ] N8N accessible at https://kabowd-n8n.fly.dev
- [ ] WAHA accessible at https://kabowd-waha.fly.dev
- [ ] WhatsApp QR scanned
- [ ] N8N ↔ WAHA connection tested
- [ ] Auto-deploy verified (push to GitHub, Fly redeploys)
- [ ] First workflow created & running

---

## 🎯 Workflow

Now your workflow is:

1. **Make changes locally**
   ```bash
   # Edit files, test locally
   git add .
   git commit -m "Update"
   git push origin main
   ```

2. **Fly.io auto-deploys**
   - GitHub sends webhook to Fly.io
   - Fly.io rebuilds Docker images
   - Fly.io restarts services
   - Apps are live in 2-3 minutes

3. **Check status**
   - Dashboard → Deployments
   - See logs
   - Monitor metrics

---

## 🔐 Security Notes

### ✅ DO:
- Store secrets in Fly.io **Secrets** section (not in repo)
- Use `.gitignore` to prevent credential commits
- Rotate API keys regularly

### ❌ DON'T:
- Commit passwords to GitHub
- Put secrets in fly.toml files (use Secrets section instead)
- Use default credentials in production

---

## 🆘 Troubleshooting

### "Deployment failed"
- Check Logs tab: Dashboard → App → Logs
- Common issues:
  - Dockerfile path wrong
  - Missing env variables
  - Port mismatch

### "Can't access app"
- Check Status: Dashboard → Deployments
- Wait 2-3 min after deployment
- Check firewall (shouldn't be an issue with Fly.io)

### "Secrets not working"
- Redeploy after adding secrets:
  - Dashboard → App → Settings → Restart

---

## ✨ You're Done!

Your N8N + WAHA is now:
- ✅ On GitHub (version control)
- ✅ Deployed to Fly.io (production)
- ✅ Auto-deploying on every push
- ✅ Running with HTTPS
- ✅ Zero cost per month

Just push code to GitHub, Fly.io handles the rest! 🚀
