# 🛡️ VoidChat — Ephemeral Encrypted Chat 1.0

> Zero-persistence, end-to-end encrypted chat rooms that vanish when everyone leaves.

![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-Realtime-3FCF8E?logo=supabase&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-5-646CFF?logo=vite&logoColor=white)

---
<img width="1897" height="949" alt="{11C044C9-CBF7-45E1-93BC-0D01C6A20E6E}" src="https://github.com/user-attachments/assets/28422235-3dcd-4560-a62f-e52d0520a3cc" />

<img width="1919" height="953" alt="{5C2BFA5D-5E1E-4D6E-A62A-DFFC9C676FC7}" src="https://github.com/user-attachments/assets/011bd259-76ea-47f4-831b-3c0f33835064" />



## ✨ Features

- 🔐 **End-to-end encryption** — AES-256-GCM via Web Crypto API. No plaintext ever reaches the server.
- 👻 **Fully ephemeral** — No database, no message storage. Everything lives in memory and vanishes on close.
- 🎭 **Anonymous** — No accounts, no login. Random usernames generated per session.
- ✏️ **Editable usernames** — Click your name to change it on the fly. Presence updates in real time.
- 📡 **Realtime** — Powered by Supabase Realtime Broadcast & Presence.
- 🌐 **Presence** — See who's online in real time.
- 📎 **Encrypted file sharing** — Send images and small files (JPG, PNG, GIF, WebP, TXT, DOCX up to 200KB) fully encrypted via Broadcast. No server storage.
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

- Supported formats: **JPG, PNG, GIF, WebP, TXT, DOCX**
- Max size: **200KB** after compression (Supabase Broadcast payload limit)
- **Client-side compression** runs before encryption to fit within the limit:
  - **Images** (JPG, PNG, GIF, WebP) → resized via Canvas API (max width 1400px) and re-encoded as WebP (fallback JPEG) at quality 0.75
  - **TXT / DOCX / others** → gzip compression via the native `CompressionStream` API
  - A `Compressing file...` indicator is shown during processing
  - If the result still exceeds 200KB, the upload is rejected with a clear error
- Compressed files are then converted to Base64, encrypted with AES-256-GCM, and broadcast to all participants
- **No files are ever stored on any server** — they exist only in participants' browser memory

## 💬 Message Reactions

- Tap/click any message to open a quick-reaction picker with 12 curated emojis
- Reactions are broadcast in real time and grouped by emoji
- Like everything else, reactions are **ephemeral** — they vanish when the session ends


## 🏗️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18 + TypeScript |
| Styling | Tailwind CSS + shadcn/ui |
| Build | Vite 5 |
| Realtime | Supabase Broadcast & Presence |
| Encryption | Web Crypto API (AES-256-GCM + PBKDF2) |


---

<p align="center">
  <b>VoidChat</b> — Messages that exist only in the moment. 💀
   Made with ❤️ by Aisurf3r
</p>
