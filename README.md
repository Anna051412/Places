# The Palace That Eats Souls — Complete Setup Guide

## Option A: Zero-Install (Recommended to start)

You already have the finished website as a single HTML file.

1. Download `palace_eats_souls.html` from this chat
2. Double-click it — it opens directly in Chrome, Firefox, Safari, or Edge
3. That's it. No server needed.

This file contains everything: HTML, CSS, JavaScript, fonts (loaded from Google Fonts).
It works offline except for the Google Fonts (text still shows, just falls back to Georgia).

---

## Option B: Run the Full React Project Locally (your zip file)

This runs the original React/Vite project from your zip — with hot-reload during development.

### Prerequisites
Install these once on your machine:

- **Node.js** (v18 or newer): https://nodejs.org — click "LTS" and install
- **pnpm**: After Node is installed, open Terminal/Command Prompt and run:
  ```
  npm install -g pnpm
  ```

### Steps

1. **Extract your zip file**
   Extract `palace_eats_souls.zip` to a folder, e.g. `C:\Projects\palace_site` or `~/Projects/palace_site`

2. **Open Terminal in that folder**
   - Windows: Right-click the folder → "Open in Terminal"
   - Mac: Right-click → "New Terminal at Folder"

3. **Install dependencies**
   ```
   pnpm install
   ```
   This downloads all the packages (React, Tailwind, etc.). Takes 1–2 minutes.

4. **Fix the known bugs first** (see "Bug Fixes" section below)

5. **Start the dev server**
   ```
   pnpm dev
   ```

6. **Open in browser**
   It will say something like:
   ```
   Local: http://localhost:5173/
   ```
   Open that URL. The site runs locally. Changes to any file hot-reload instantly.

### To build for production
```
pnpm build
```
This creates a `dist/` folder with optimized files you can deploy anywhere.

---

## Bug Fixes for the React Project

Apply these before running. Open each file and make the changes:

### Fix 1 — `client/src/App.tsx` (Route typo)
Find this line:
```tsx
<Route path={"/ "} component={Home} />
```
Change to:
```tsx
<Route path={"/"} component={Home} />
```
(Remove the space inside the quotes)

### Fix 2 — `client/src/pages/Home.tsx` (Broken nav HTML)
Find this broken block around lines 45–55:
```tsx
<a href="#descent" className="block text-sm font-mono hover:text-accent transition-colors">
<a href="/throne-room" className="block text-sm font-mono hover:text-accent transition-colors">
  → Enter the Throne Room
</a>
  → Descend
</a>
```
Replace with:
```tsx
<a href="/descent" className="block text-sm font-mono hover:text-accent transition-colors">
  → Descend
</a>
<a href="/throne-room" className="block text-sm font-mono hover:text-accent transition-colors">
  → Enter the Throne Room
</a>
```

### Fix 3 — `client/src/components/AmbientAudio.tsx` (Scroll bug)
In the scroll handler function, find:
```tsx
const targetVolume = scrollProgress * 0.05;
```
The line above it uses `scrollY` from state instead of the current value. Replace the entire handler with:
```tsx
const handleScroll = () => {
  if (!isInitialized || gainsRef.current.length === 0) return;
  const maxScroll = document.documentElement.scrollHeight - window.innerHeight;
  const currentScroll = window.scrollY;
  const scrollProgress = Math.min(currentScroll / maxScroll, 1);
  const targetVolume = scrollProgress * 0.05;
  gainsRef.current.forEach((gain) => {
    gain.gain.linearRampToValueAtTime(targetVolume, audioContextRef.current!.currentTime + 0.5);
  });
};
```

### Fix 4 — Nav hover zone (UX fix)
In `client/src/pages/Home.tsx`, the nav dropdown closes before you can click links.
Change the nav container from:
```tsx
<div className="group cursor-pointer" onMouseEnter=... onMouseLeave=...>
  <div>...</div>
  {isHoveringNav && <div className="absolute top-8 right-0 ...">...</div>}
</div>
```
To wrap both the trigger and dropdown in one hover zone — or simply add `onMouseEnter` to the dropdown div too so it doesn't close when moving the mouse into it.

---

## Option C: Host on GitHub Pages (Free, shareable URL)

This gives you a real URL like `https://yourusername.github.io/palace/`

### Using the single HTML file (easiest):

1. Create a free account at https://github.com
2. Click "New repository" → name it `palace` → set it to Public → Create
3. Click "Add file" → "Upload files" → upload `palace_eats_souls.html`
4. Rename it to `index.html` (GitHub Pages serves `index.html` by default)
5. Go to Settings → Pages → Source: "Deploy from branch" → Branch: main → Save
6. Wait ~2 minutes → your site is live at `https://yourusername.github.io/palace/`

### Using the React project (for the full version):

1. Install the `gh-pages` package:
   ```
   pnpm add -D gh-pages
   ```
2. In `package.json`, add to scripts:
   ```json
   "deploy": "gh-pages -d dist"
   ```
3. In `vite.config.ts`, add:
   ```ts
   base: '/palace/',
   ```
4. Build and deploy:
   ```
   pnpm build
   pnpm deploy
   ```
5. In GitHub repo Settings → Pages → set source to `gh-pages` branch

---

## Option D: Deploy on Vercel (Best for React, free tier)

Vercel gives you a real URL with HTTPS in ~2 minutes.

1. Push your project to GitHub (create a repo, `git init`, `git add .`, `git commit -m "init"`, `git push`)
2. Go to https://vercel.com → Sign up with GitHub
3. Click "Add New Project" → Import your GitHub repo
4. Vercel auto-detects Vite. Leave all settings as-is.
5. Click Deploy → done. URL is something like `https://palace-eats-souls.vercel.app`

Every `git push` auto-deploys.

---

## Quick Reference: What Each Page Does

| URL / Page | What it is |
|---|---|
| `/` (Home) | Main landing — hero, story sections, depth tracker, truth reveal |
| `/descent` | The Palace interior — 5 reveal sections, each hides lore behind [REVEAL] |
| `/throne-room` | Branching narrative — 3 choices → 4 unique endings |

## Features in the HTML file

- Custom animated cursor (dot + lagging ring)
- Scroll progress bar at top
- Ambient drone audio (Web Audio API, toggle bottom-right)
- Scroll-depth tracker with animated lines
- Breathing background pulse
- Grid parallax in hero
- Glitch animation on "That Eats"
- Fade-in-on-scroll for all sections
- Full branching narrative with 4 endings
- 5 reveal sections with open/close
- Mobile responsive with hamburger nav
- Zero dependencies
