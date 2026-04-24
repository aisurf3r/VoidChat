# 🛡️ VoidChat — Ephemeral Encrypted Chat (work in progress)

> Zero-persistence, end-to-end encrypted chat rooms that vanish when everyone leaves.

![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-Realtime-3FCF8E?logo=supabase&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-5-646CFF?logo=vite&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

## ✨ Features

- 🔐 **End-to-end encryption** — AES-256-GCM via Web Crypto API. No plaintext ever reaches the server.
- 👻 **Fully ephemeral** — No database, no message storage. Everything lives in memory and vanishes on close.
- 🎭 **Anonymous** — No accounts, no login. Random usernames generated per session.
- ✏️ **Editable usernames** — Click your name to change it on the fly. Presence updates in real time.
- 📡 **Realtime** — Powered by Supabase Realtime Broadcast & Presence.
- 🌐 **Presence** — See who's online in real time.
- 📎 **Encrypted file sharing** — Send images and small files (JPG, PNG, GIF, WebP, PDF, TXT up to 200KB) fully encrypted via Broadcast. No server storage.
- 🌗 **Light & Dark mode** — Toggle between dark cyberpunk and clean light themes. Preference is saved locally.
- 🎨 **Cyberpunk UI** — Dark theme with glow effects and smooth animations.
- 📱 **Mobile optimized** — Responsive layout using dynamic viewport height for a native-like feel on phones.

## 🔒 How Encryption Works

```
Room Code → PBKDF2 (100k iterations, SHA-256) → AES-256 Key
Message → AES-GCM Encrypt → Base64 → Broadcast Channel → Decrypt → Display
```

1. When a room is created, a random 7-character code is generated.
2. The code is used to derive a **256-bit AES key** via PBKDF2.
3. Every message and file attachment is **encrypted client-side** before being sent through Supabase Broadcast.
4. The key exists **only in `sessionStorage`** — it disappears when the tab closes.
5. **Supabase never sees plaintext.** It only relays encrypted payloads.

## 📎 File Sharing

Files are shared using the same encryption channel as messages:

- Supported formats: **JPG, PNG, GIF, WebP, PDF, TXT**
- Max size: **200KB** (constrained by Supabase Broadcast limits)
- Files are converted to Base64, encrypted with AES-256-GCM, and broadcast to all participants
- **No files are ever stored on any server** — they exist only in participants' browser memory

## 🚀 Self-Hosting Guide

### Prerequisites

- [Node.js](https://nodejs.org/) 18+ (or [Bun](https://bun.sh/))
- A free [Supabase](https://supabase.com/) account

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/voidchat.git
cd voidchat
```

### 2. Create a Supabase project

1. Go to [supabase.com](https://supabase.com/) and create a free account.
2. Click **"New Project"** and choose a name and region.
3. Wait for it to finish setting up (~1 minute).
4. Go to **Settings → API** and copy:
   - **Project URL** (looks like `https://xxxxx.supabase.co`)
   - **anon/public key** (a long `eyJ...` string)

> ⚠️ You do NOT need to create any database tables. VoidChat only uses Supabase's **Realtime** feature (Broadcast & Presence), which works out of the box.

### 3. Configure environment variables

Create a `.env` file in the project root:

```env
VITE_SUPABASE_URL=https://YOUR_PROJECT_ID.supabase.co
VITE_SUPABASE_PUBLISHABLE_KEY=eyJhbGci...your_anon_key_here
```

### 4. Install dependencies and run

```bash
# Using npm
npm install
npm run dev

# Or using Bun (faster)
bun install
bun dev
```

The app will be available at `http://localhost:5173` 🎉

### 5. Build for production

```bash
npm run build
# Output will be in the `dist/` folder
```

You can deploy the `dist/` folder to any static hosting:
- **Vercel**: `npx vercel`
- **Netlify**: drag & drop the `dist/` folder
- **Cloudflare Pages**: connect your repo
- **GitHub Pages**: use `gh-pages` package
- **Any web server**: just serve the `dist/` folder as static files

## 🏗️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18 + TypeScript |
| Styling | Tailwind CSS + shadcn/ui |
| Build | Vite 5 |
| Realtime | Supabase Broadcast & Presence |
| Encryption | Web Crypto API (AES-256-GCM + PBKDF2) |

## 📁 Project Structure

```
src/
├── components/
│   ├── ChatRoom.tsx      # Main chat interface with file sharing
│   ├── RoomEntry.tsx      # Create/join room screen
│   └── ui/                # shadcn/ui components
├── hooks/
│   └── useTheme.ts        # Light/dark mode toggle hook
├── integrations/
│   └── supabase/
│       └── client.ts      # Supabase client (reads env vars)
├── lib/
│   └── crypto.ts          # Encryption utilities
├── pages/
│   └── Index.tsx           # Main page
└── index.css               # Design tokens & theme (light + dark)
```

## 🤝 Contributing

Contributions are welcome! Feel free to:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/cool-thing`)
3. Commit your changes (`git commit -m 'Add cool thing'`)
4. Push to the branch (`git push origin feature/cool-thing`)
5. Open a Pull Request

## 📄 License

MIT — do whatever you want with it.

---

<p align="center">
  <b>VoidChat</b> — Messages that exist only in the moment. 💀
</p>
