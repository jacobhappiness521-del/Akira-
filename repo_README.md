# ⚡ AKIRA — Anime AI Chatbot

> Your ultimate AI-powered anime expert. Ask about characters, power systems, arcs, lore, and rankings across 40+ anime series.

![AKIRA Chatbot](https://img.shields.io/badge/AKIRA-Anime%20AI%20Chatbot-ff0077?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0id2hpdGUiIGQ9Ik03IDJsNiAxMC01IDEwaDEwTDEyIDEyeiIvPjwvc3ZnPg==)
![Built with React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react)
![Powered by Claude](https://img.shields.io/badge/Claude-Sonnet-orange?style=for-the-badge)
![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-Ready-brightgreen?style=for-the-badge)

---

## 🌐 Live Demo

**[→ Open AKIRA Chatbot](https://YOUR_USERNAME.github.io/anime-chatbot)**

*(Replace with your actual GitHub Pages URL after deployment)*

---

## ✨ Features

- 🤖 **AI-Powered** — Uses Claude claude-sonnet-4-20250514 for deep, accurate anime knowledge
- 🔑 **Secure API Key Input** — Users enter their own Anthropic key; stored in session memory only
- 📚 **40+ Anime** — Deep coverage of Demon Slayer, JJK, Solo Leveling, Frieren, HxH, JoJo's, and more
- ⚡ **Real-time Chat** — Smooth streaming-style conversation
- 🎌 **Anime Library** — Clickable list of all supported series
- 💡 **Quick Suggestions** — One-tap conversation starters
- 📱 **Fully Responsive** — Works on mobile and desktop
- 🌙 **Cyberpunk Dark Theme** — Katakana rain, glowing accents, anime aesthetic

---

## 🚀 Deploy to GitHub Pages (3 Steps)

### Step 1 — Fork or Clone this repo

```bash
# Clone
git clone https://github.com/YOUR_USERNAME/anime-chatbot.git
cd anime-chatbot

# OR fork via GitHub UI, then clone your fork
```

### Step 2 — Enable GitHub Pages with Actions

1. Go to your repo on GitHub
2. Click **Settings** → **Pages**
3. Under **Source**, select **GitHub Actions**
4. Click **Save**

### Step 3 — Push to main branch

```bash
git add .
git commit -m "Deploy AKIRA chatbot"
git push origin main
```

The GitHub Action will automatically build and deploy your site. In ~2 minutes, your chatbot will be live at:
```
https://YOUR_USERNAME.github.io/anime-chatbot
```

---

## 💻 Local Development

```bash
# Install dependencies
npm install

# Start dev server (hot reload)
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

---

## 🔑 Getting an Anthropic API Key

1. Visit [console.anthropic.com](https://console.anthropic.com)
2. Create an account (free tier available)
3. Go to **API Keys** → **Create Key**
4. Copy the key (starts with `sk-ant-`)
5. Paste it into AKIRA's setup screen

> **Privacy**: The API key is stored in `sessionStorage` only — it lives in your browser tab and is deleted when you close it. It is never sent anywhere except directly to Anthropic's API.

---

## 📁 Project Structure

```
anime-chatbot/
├── .github/
│   └── workflows/
│       └── deploy.yml        # Auto-deploy to GitHub Pages
├── public/
│   └── favicon.svg           # ⚡ AKIRA icon
├── src/
│   ├── main.jsx              # React entry point
│   ├── index.css             # Global styles
│   ├── App.jsx               # Root — routes between Setup & Chat
│   ├── ApiKeySetup.jsx       # Beautiful API key entry screen
│   └── Chatbot.jsx           # Main chat interface
├── index.html                # HTML shell
├── vite.config.js            # Vite configuration
├── package.json              # Dependencies & scripts
└── .gitignore
```

---

## 🎌 Anime Knowledge Covers

| Category | Anime |
|----------|-------|
| **Dark Fantasy** | Demon Slayer, Hell's Paradise, Akame ga Kill, Tougen Anki |
| **Modern Hits** | Jujutsu Kaisen, Frieren, Solo Leveling, Kaiju No. 8 |
| **Sports** | Blue Lock, Blue Box, Haikyuu |
| **Classics** | Naruto, Bleach, One Piece, Dragon Ball Z, Death Note |
| **Psychological** | Steins;Gate, Code Geass, Classroom of the Elite, Re:Zero |
| **Isekai** | Overlord, TenSura, Mushoku Tensei, No Game No Life |
| **Ghibli** | Spirited Away (and Studio Ghibli filmography) |
| **Unique** | JoJo's Bizarre Adventure, One Punch Man, Spy x Family, HxH |
| **Donghua** | Daily Life of the Immortal King |

---

## 🛠️ Customization

### Change the AI personality
Edit `SYSTEM_PROMPT` in `src/Chatbot.jsx`:

```js
const SYSTEM_PROMPT = `You are AKIRA — ...your custom prompt here...`
```

### Add more anime to the library
Edit `ANIME_LIST` in `src/Chatbot.jsx`:

```js
const ANIME_LIST = [
  'Spirited Away',
  'Your New Anime Here',  // ← add here
  ...
]
```

### Change colors/theme
The color variables used throughout:
- `#4cc9f0` — Cyan (AKIRA primary)
- `#f72585` — Pink (user accent)
- `#7209b7` — Purple (secondary)
- `#4361ee` — Blue (gradient)
- `#0a0a0f` — Dark background

---

## ⚙️ Tech Stack

| Tool | Purpose |
|------|---------|
| [React 18](https://react.dev) | UI framework |
| [Vite 5](https://vitejs.dev) | Build tool & dev server |
| [Anthropic Claude API](https://anthropic.com) | AI responses |
| [GitHub Actions](https://github.com/features/actions) | Auto-deployment |
| [GitHub Pages](https://pages.github.com) | Free hosting |

---

## 📄 License

MIT — free to use, modify, and distribute.

---

*Built with ⚡ for anime fans everywhere*
