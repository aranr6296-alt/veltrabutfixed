# Discord Bot — Railway Deployment Guide

## 🚀 Deploy to Railway (Step-by-Step)

### 1. Create a new Railway project
Go to [railway.app](https://railway.app) → **New Project** → **Deploy from GitHub repo**
(or drag-and-drop this folder using the Railway CLI).

### 2. Add a PostgreSQL database
In your Railway project → **New** → **Database** → **Add PostgreSQL**.
Railway will automatically inject `DATABASE_URL` into your service's environment.

### 3. Set environment variables
In your Railway service → **Variables** tab, add:

| Variable | Value |
|---|---|
| `DISCORD_TOKEN` | Your bot token from [discord.com/developers](https://discord.com/developers/applications) |
| `DATABASE_URL` | Auto-filled by Railway when you add Postgres |

### 4. Deploy
Railway will detect `nixpacks.toml` and automatically:
- Install Python 3.12 + FFmpeg
- Run `pip install -r requirements.txt` (installs `psycopg2-binary` ✅)
- Start the bot with `python main.py`

---

## ⚙️ What was fixed

| Problem | Fix |
|---|---|
| `ModuleNotFoundError: No module named 'psycopg2'` | Added `requirements.txt` with `psycopg2-binary` |
| FFmpeg not found (music commands) | Added `ffmpeg` to `nixpacks.toml` |
| Bot crashes on restart | Added `restartPolicyType = "ON_FAILURE"` in `railway.toml` |
| No `.env` on Railway | All secrets go in Railway's **Variables** tab |

---

## 📁 File structure

```
discord-bot/
├── main.py            ← The full bot code
├── requirements.txt   ← Python dependencies
├── Procfile           ← Tells Railway how to run the bot
├── railway.toml       ← Railway deploy config
├── nixpacks.toml      ← Build config (Python + FFmpeg)
├── .env.example       ← Template — copy to .env for local dev
└── README.md          ← This file
```

## 🖥️ Local development

```bash
cp .env.example .env
# Fill in DISCORD_TOKEN and DATABASE_URL in .env

pip install -r requirements.txt
python main.py
```
