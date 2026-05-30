<div align="center">

<img src="https://img.shields.io/badge/XEN-AI%20Assistant-white?style=for-the-badge&labelColor=black" />

**A minimal, self-hosted AI chat assistant**
Built with n8n · Docker · Cloudflare Tunnel · GitHub Pages

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Visit%20XEN-brightgreen?style=flat-square)](https://divyanshhhsain.github.io/XEN)
[![Status](https://img.shields.io/badge/Status-Self%20Hosted-blue?style=flat-square)]()
[![Built With](https://img.shields.io/badge/Stack-n8n%20%7C%20Docker%20%7C%20Cloudflare-orange?style=flat-square)]()
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

</div>

---

## 🖥️ Preview

> XEN features a clean dark landing page where you type your first message, which instantly transitions into a smooth chat interface.

---

## ✨ What is XEN?

XEN is a fully self-hosted AI chatbot assistant with a clean, minimal interface. It runs entirely on your local machine — no paid cloud services, no third-party AI billing per message. The frontend lives on GitHub Pages and talks to an n8n workflow engine running in Docker on your PC, exposed to the internet through Cloudflare Tunnel.

---

## 🚀 Features

- 💬 **Smooth chat UI** — landing page transitions into a full chat view
- ⚡ **Real-time responses** — typing indicator while XEN is thinking
- 🔄 **Session memory** — maintains conversation context via session ID
- 🎨 **Minimal dark design** — Sora font, clean dark theme, responsive layout
- 🐳 **Dockerized backend** — n8n runs in a container, fully isolated
- 🌍 **Publicly accessible** — Cloudflare Tunnel exposes local n8n securely
- 🔒 **CORS secured** — only your GitHub Pages domain can call the webhook
- 📱 **Mobile friendly** — works on any device, any screen size
- ⚙️ **Auto URL updater** — PowerShell script auto-updates GitHub when tunnel restarts

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Frontend | HTML + CSS + JavaScript | Chat UI, single file |
| Font | Sora (Google Fonts) | Clean typography |
| Hosting | GitHub Pages | Public frontend hosting |
| Workflow Engine | n8n (self-hosted) | AI workflow automation |
| Containerization | Docker | Runs n8n locally |
| Public Tunnel | Cloudflare Tunnel | Exposes local n8n publicly |
| Automation | PowerShell | Auto-start + URL updater |

---

## 🏗️ Architecture

```
User (any device, anywhere)
          │
          ▼
  GitHub Pages / Netlify
  divyanshhhsain.github.io/XEN
          │
          │  POST /webhook/xen
          │  { chatInput, message, sessionId }
          ▼
  Cloudflare Tunnel
  (*.trycloudflare.com)
          │
          ▼
  n8n Workflow Engine
  Docker Container — localhost:5678
          │
          ▼
  AI Model → Response
          │
          ▼
  Back to User Browser
```

---

## ⚙️ How to Run Locally

### Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running
- [cloudflared](https://github.com/cloudflare/cloudflared/releases/latest) installed
- Windows / Mac / Linux

### Step 1 — Clone the repo
```bash
git clone https://github.com/DivyanshhhSain/XEN.git
cd XEN
```

### Step 2 — Create docker-compose.yml
```yaml
version: '3.8'
services:
  n8n:
    image: n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_HOST=0.0.0.0
      - N8N_PORT=5678
      - N8N_PROTOCOL=https
      - N8N_CORS_ALLOWED_ORIGINS=https://divyanshhhsain.github.io
      - N8N_CORS_ALLOW_METHODS=GET,POST,OPTIONS
      - N8N_CORS_ALLOW_HEADERS=Content-Type,Authorization
    volumes:
      - n8n_data:/home/node/.n8n
volumes:
  n8n_data:
```

### Step 3 — Start n8n
```bash
docker compose up -d
```

### Step 4 — Start Cloudflare Tunnel
```bash
cloudflared tunnel --url http://localhost:5678
```

### Step 5 — Update webhook URL
Copy the `trycloudflare.com` URL and update this line in `index.html`:
```javascript
const response = await fetch("https://beginners-disc-cookie-fellowship.trycloudflare.com/webhook/xen", {
```

### Step 6 — Activate your n8n workflow
Open `http://localhost:5678`, build or import your AI workflow, and toggle it **Active**.

---

## 📁 Project Structure

```
XEN/
├── index.html       ← Entire app (UI + logic in one file)
├── netlify.toml     ← Netlify deployment config
├── LICENSE          ← MIT License
└── README.md        ← This file
```

---

## 💡 How the Frontend Works

1. User lands on the **landing page** — minimal input, clean centered layout
2. On first message → **transitions to chat view** with smooth animation
3. Message is sent as `POST` to n8n webhook with `chatInput`, `message`, and `sessionId`
4. **Typing indicator** (3 bouncing dots) shows while waiting for response
5. Response is extracted from n8n output and displayed as assistant message
6. Session ID persists in `sessionStorage` for conversation memory

---

## 🔮 Roadmap

- [x] Landing page → chat transition
- [x] Real-time typing indicator
- [x] Session-based memory
- [x] Docker + n8n backend
- [x] Cloudflare Tunnel integration
- [x] CORS configuration
- [x] Auto tunnel URL updater (PowerShell)
- [x] Netlify deployment
- [ ] Voice input / text-to-speech
- [ ] Chat history persistence
- [ ] Multiple AI model switching
- [ ] Mobile app (PWA)
- [ ] Always-online cloud deployment

---

## ⚠️ Notes

XEN is **self-hosted** — it runs on a personal machine. This means:
- It's live when the host PC is on and the tunnel is running
- The Cloudflare tunnel URL changes on restart (auto-updated by script)
- For always-on deployment, a cloud server is needed (coming in v2)

---

## 👨‍💻 Author

**Divyanshu Sain**
- GitHub: [@DivyanshhhSain](https://github.com/DivyanshhhSain)

---

## 📄 License

This project is open source under the [MIT License](LICENSE).

---

<div align="center">
  <sub>Built from scratch by Divyanshu Sain · Star ⭐ if you liked it!</sub>
</div>
