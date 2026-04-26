# Deployment Guide

This guide covers deployment options for the Stellar Bounty Board.

- [Render (Backend) & Vercel (Frontend)](#render-vercel)
- [Railway One-Click Deployment](#railway)

---

## Render & Vercel <a name="render-vercel"></a>

### Backend Deployment: Render

1. Go to [Render](https://render.com/) and click **New Web Service**.
2. Connect your fork of this repo and select `backend/` as the root directory.
3. Build command: `npm install && npm run build`
4. Start command: `npm start`
5. Health check path: `/api/health`
   - A healthy response: `{ "service": "stellar-bounty-board-backend", "status": "ok", ... }`
6. Ensure the port is set to `3001` (or use `process.env.PORT` as Render provides).

### Frontend Deployment: Vercel

1. Go to [Vercel](https://vercel.com/) and import your fork of this repo.
2. Set the root directory to `frontend/`.
3. Build command: `npm run build`
4. Output directory: `dist`
5. Environment variable: `VITE_API_URL` set to your Render backend URL (e.g., `https://your-backend.onrender.com/api`).

---

## Railway One-Click Deployment <a name="railway"></a>

[Railway](https://railway.com/) is a popular alternative to Render that offers one-click GitHub repo deployment. It auto-detects Node.js projects and provides generous free tier limits.

### Prerequisites

- A [Railway](https://railway.com/) account (sign up with GitHub)
- Your fork of this repository

### Deployment Steps

#### Quick Deploy (Recommended)

[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template?template_url=https://github.com/ritik4ever/stellar-bounty-board)

Click the button above or follow the manual steps below.

#### Manual Deployment

1. **Create a New Project**
   - Go to [Railway Dashboard](https://railway.com/dashboard)
   - Click **New Project** → **Deploy from GitHub repo**
   - Select your fork of `stellar-bounty-board`

2. **Configure the Backend Service**
   - Railway will auto-detect the Node.js project in the root
   - Set the **Root Directory** to `backend/`
   - Build command (auto-detected): `npm install && npm run build`
   - Start command (auto-detected): `npm start`
   - Railway assigns a `${{PORT}}` environment variable automatically (overrides the default `3001`)

3. **Set Environment Variables**
   In the Railway dashboard, navigate to your service → **Variables** and add:

   | Variable | Required | Example Value | Description |
   |----------|----------|---------------|-------------|
   | `GITHUB_WEBHOOK_SECRET` | ✅ Yes | `your_webhook_secret` | Secret for GitHub webhook verification |
   | `NODE_ENV` | ❌ No | `production` | Environment mode |
   | `CORS_ORIGINS` | ❌ No | `https://your-frontend.vercel.app` | Allowed CORS origins |
   | `BOUNTY_STORE_PATH` | ❌ No | `./data/bounties.json` | Bounty data file (default) |
   | `BOUNTY_AUDIT_STORE_PATH` | ❌ No | `./data/audit.json` | Audit log file (default) |
   | `LOG_LEVEL` | ❌ No | `info` | Logging level |

   > See [`.env.example`](../.env.example) for the full list of environment variables.

4. **Set Up the Frontend (Optional)**
   - Create a **second service** in the same Railway project
   - Set the **Root Directory** to `frontend/`
   - Add environment variable: `VITE_API_URL` pointing to your deployed backend URL
   - Railway will auto-detect and build the Vite project

5. **Configure a Custom Domain**
   - Railway provides a `*.railway.app` domain automatically
   - For a custom domain: Settings → Domains → Add your domain
   - Railway handles SSL/TLS certificates automatically

6. **Set Up Health Check**
   - Health check path: `/api/health`
   - Railway monitors this and restarts the service if it fails
   - Expected response: `{ "service": "stellar-bounty-board-backend", "status": "ok", ... }`

### Railway-Specific Tips

- **Automatic Deploys**: Railway auto-deploys when you push to your default branch
- **Logs**: View real-time logs in the Railway dashboard (Deployments → Logs)
- **Rollbacks**: Click on any past deployment to redeploy it
- **Free Tier**: Includes \$5 of free credits monthly — enough for a small project
- **Sleep Policy**: Services sleep after periods of inactivity (can be disabled in Settings)

### Troubleshooting

| Issue | Solution |
|-------|----------|
| Build fails with Node version | Set `NODE_VERSION` environment variable to `18` or `20` |
| Service crashes on start | Check logs for missing environment variables |
| Frontend can't reach backend | Verify `VITE_API_URL` points to the correct Railway backend URL |
| CORS errors | Set `CORS_ORIGINS` to your frontend domain |
| Health check failing | Ensure `/api/health` returns JSON with `status: "ok"` |

---

## Required Environment Variables

### Backend
- `GITHUB_WEBHOOK_SECRET` — Required for webhook verification
- All other variables have sensible defaults (see `.env.example`)

### Frontend
- `VITE_API_URL` — URL of your deployed backend API

---

## Health Check Paths

- Backend: `/api/health` (should return `{ "status": "ok", ... }`)
- Frontend: `/` (should load the React dashboard)

---

## Common Deployment Issues & Fixes

- **Build fails:** Check Node.js version (18+), install all dependencies, and verify build commands.
- **API not reachable:** Confirm backend is live and CORS is configured.
- **Env vars not set:** Double-check environment variable names and values.
- **Frontend shows blank:** Ensure correct output directory (`dist`) and that the API URL is set.

---

## Need Help?

- Check the [ONBOARDING.md](../ONBOARDING.md) for local setup.
- Open an issue or discussion in the repo for deployment help.
