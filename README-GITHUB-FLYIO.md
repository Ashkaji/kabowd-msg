# 🚀 Kabowd - N8N + WAHA on Fly.io

Production-grade WhatsApp automation on Fly.io using N8N workflows.

## 📋 Quick Setup (5 minutes)

### 1. Push to GitHub
```bash
git init
git add .
git commit -m "Initial commit: N8N + WAHA for Fly.io"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/kabowd-automation.git
git push -u origin main
```

### 2. Deploy on Fly.io Dashboard
1. Go to https://fly.io/dashboard
2. Click **"Create New App"**
3. Choose **"Deploy from GitHub"**
4. Select your repo
5. Follow: [`FLY-IO-GITHUB-DEPLOY.md`](./FLY-IO-GITHUB-DEPLOY.md) Steps 3-8

### 3. Done! 🎉
```
N8N:  https://kabowd-n8n.fly.dev
WAHA: https://kabowd-waha.fly.dev/api/dashboard
```

---

## 📚 Documentation

- **[FLY-IO-GITHUB-DEPLOY.md](./FLY-IO-GITHUB-DEPLOY.md)** ← START HERE
  - Step-by-step web dashboard setup
  - No CLI required
  - Screenshots & instructions
  - Auto-deploy on GitHub push

---

## 📦 Project Structure

```
kabowd-automation/
├── FLY-IO-GITHUB-DEPLOY.md     📖 Complete deployment guide
├── fly-n8n.toml                ⚙️ N8N Fly configuration
├── fly-waha.toml               ⚙️ WAHA Fly configuration
├── docker-compose.yaml         (local development)
├── .gitignore                  🔒 Protect credentials
├── README-GITHUB-FLYIO.md      (this file)
├── n8n/
│   └── Dockerfile              N8N container
└── waha/
    └── Dockerfile              WAHA container
```

---

## 🎯 Workflow

```
1. Make changes locally
   ↓
2. git add . && git commit && git push
   ↓
3. GitHub sends webhook to Fly.io
   ↓
4. Fly.io auto-builds Docker images
   ↓
5. Fly.io restarts services
   ↓
6. Apps live in 2-3 minutes ✅
```

---

## 💰 Cost

| Service | Cost |
|---|---|
| N8N on Fly.io | $0/month |
| WAHA on Fly.io | $0/month |
| HTTPS/Bandwidth | $0/month |
| **TOTAL** | **$0/month** |

(Free $5/month credit covers both)

---

## 🔐 Security

- Secrets stored in **Fly.io Secrets** (not in repo)
- `.gitignore` prevents credential leaks
- HTTPS by default
- No passwords in GitHub

---

## 📖 Next Steps

👉 **Read [`FLY-IO-GITHUB-DEPLOY.md`](./FLY-IO-GITHUB-DEPLOY.md) for complete setup**

1. Step 1: Push code to GitHub ✅
2. Step 2-8: Follow web dashboard instructions
3. Done! Auto-deploy on every push

---

## 🆘 Issues?

- **Deployment failed?** → Check Logs in Fly.io dashboard
- **Can't access app?** → Wait 2-3 min, check Status
- **Need help?** → See troubleshooting in FLY-IO-GITHUB-DEPLOY.md

---

## ✨ Features

- ✅ N8N workflow automation
- ✅ WAHA WhatsApp HTTP API
- ✅ HTTPS/SSL included
- ✅ Auto-deploy from GitHub
- ✅ Zero server management
- ✅ $0/month cost
- ✅ Production-ready

---

Made for **Kabowd Meetup 2026** 🎉
