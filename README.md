# 🛡️ DriveVault — Connected App Data Extractor

> Audit every app that touches your Google Drive. Inspect files, review permissions, detect sensitive data, and export evidence packages — all client-side with a premium, zero-backend glassmorphic forensics console.

🚀 **Live Deployment:** [https://satiricalguru.github.io/DriveVault/](https://satiricalguru.github.io/DriveVault/)

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Status](https://img.shields.io/badge/status-active-brightgreen)
![No Backend](https://img.shields.io/badge/backend-none-green)
![Google Drive API](https://img.shields.io/badge/API-Google%20Drive%20v3-4285F4?logo=googledrive)

---

## What is DriveVault?

Third-party apps connected to your Google account quietly write files to your Drive — sync states, backups, configs, tokens — without any visibility in the standard Google Drive UI. DriveVault surfaces all of it in a beautiful, premium security-themed dashboard.

DriveVault is a **single-file, zero-backend web app** that connects to your Google account via OAuth 2.0 and lets you:

- **Audit Connected Apps:** View every file that connected apps have written to your Drive.
- **Inspect Payloads:** Browse raw file contents using Monaco Editor for text/JSON, an inline image viewer, or a custom-built hex dump viewer for binary assets.
- **Vulnerability Diagnostics:** Detect sensitive keys (passwords, sessions, coordinates, tokens) inside your files automatically.
- **Export Evidence Packages:** Package selected logs or files into a ZIP export, a metadata CSV catalog, or a compiled JSON forensics report.
- **Live Monitoring Log:** Watch for live creations and modifications on your Drive with a background watcher polling the Changes API.
- **Custom settings manager:** Enter and save your Google Client ID and Anthropic Proxy URL directly in the browser's `localStorage` — no code edits required!
- **Interactive simulated console:** Preview the entire forensic dashboard directly on the landing page before connecting.

---

## Honest Scope — What DriveVault Can and Cannot Do

| Capability | Available | How |
|---|---|---|
| Files apps saved to your main Drive | ✅ | `drive.readonly` + metadata scan |
| App permission & activity audit | ✅ | Drive Activity API + token introspection |
| DriveVault's own `appDataFolder` | ✅ | `drive.appdata` scope |
| Other apps' private `appDataFolder` | ❌ | Google hard-sandboxes this — no API, no workaround |

> Google enforces app-level isolation on the `appDataFolder`. Each app gets its own private bucket that no other app can read. This is by design and cannot be bypassed. DriveVault is transparent about this in the UI.

---

## Features

### 🔍 Connected Apps Auditor
Groups files by the app that created or modified them. Each app card features a custom risk badge, permission metadata, and stats (files count, storage audited, last activity date).

### 📁 File Explorer
Browse all app-attributed files with a tree-structured sidebar grouped by inferred app name. It supports real-time search, sorting, and type badge filters.

### 🧪 File Inspector
- **Metadata** — File ID, MIME type, size, timestamps, `appProperties`, capabilities, and Monaco Editor raw JSON view.
- **Content Tab** — Monaco Editor for text/JSON, inline image preview, and detailed custom hex viewer for binary files. It raises visual alerts if sensitive identifiers are scanned.
- **AI Analysis** — Claude-powered forensic explanation of what the file is, what app wrote it, and privacy risks (requires proxy configuration; falls back to local heuristics terminal).

### 📊 Analytics Dashboard
Donut chart of storage by app, bar chart of files per app, MIME type breakdown, list of largest files, and a modification timeline heatmap.

### 📦 Bulk Export
- **Download ZIP** — all selected files + per-file metadata JSON + manifest.
- **Metadata CSV** — spreadsheet-ready file list.
- **Full JSON Report** — metadata + base64 content for text/JSON files.

### 🔴 Live Watcher
Polls Drive every 30 seconds. Displays a live changes badge in the header and a real-time event log tab. Prevents selection reset on poll updates for a seamless user experience.

### 🔒 Privacy Mode
Disable all Claude API calls. Everything stays in the browser tab.

---

## Setup & Run

### 1. Clone the Repository

Clone the project from GitHub and navigate into the folder:

```bash
git clone https://github.com/satiricalguru/DriveVault.git
cd DriveVault
```

### 2. Get a Google OAuth Client ID

See the full walkthrough in [`docs/SETUP.md`](docs/SETUP.md) (or follow the setup instructions in the in-app configuration helper).

Short version:
1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a project → enable **Google Drive API** and **Drive Activity API**.
3. OAuth consent screen → add the six scopes listed below.
4. **Credentials** → Create **OAuth 2.0 Client ID** → Web Application.
5. Add `http://localhost:5173` (or your staging/production domain) to **Authorized JavaScript Origins**.
6. Copy the Client ID.

### 3. Configure and Run

Because DriveVault is a single-file static app, you can serve it with any lightweight server:

```bash
# Start a simple Python server
python3 -m http.server 5173
```

Then:
1. Visit [http://localhost:5173](http://localhost:5173).
2. Click **Configuration Setup** (or the gear icon).
3. Paste your Google OAuth Client ID and save. Credentials will be safely persisted in your browser's local storage.
4. Click **Connect Google Account** to authorize and begin scanning!

*Alternatively, you can open `index.html` and hardcode your client ID into `window.DRIVEVAULT_CONFIG.googleClientId`.*

---

## Required OAuth Scopes

| Scope | Why it's needed |
|---|---|
| `drive.appdata` | Read/write DriveVault's own private `appDataFolder` |
| `drive.metadata.readonly` | See metadata of all files, including app-created ones |
| `drive.readonly` | Download and read file content |
| `drive.activity.readonly` | Surface automated and app-like Drive activity |
| `userinfo.profile` | Show your name and avatar |
| `userinfo.email` | Show connected account email |

---

## Security & Privacy

- **No backend.** DriveVault is 100% client-side JavaScript.
- **Token in memory only.** The Google access token is never written to local storage, cookies, or session cache. It exists only in the active JS runtime heap and vanishes when you close the tab.
- **No data leaves your browser** except for requests directly to Google's APIs (and Anthropic's API if analysis proxy is configured).
- **Disconnect at any time.** Click Disconnect and the token is cleared. You can also revoke DriveVault's Google permission at [myaccount.google.com/permissions](https://myaccount.google.com/permissions).

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | React 18 (Babel standalone, no build step) |
| Styling | Tailwind CSS (CDN) + Glassmorphism system |
| Auth | Google Identity Services (GSI) token client flow |
| Drive API | Google Drive API v3 (raw `fetch` with backoff) |
| Activity API | Google Drive Activity API v2 |
| Code Viewer | Monaco Editor (CDN) |
| Charts | Chart.js (CDN) |
| Export | JSZip (CDN) |
| Fonts | JetBrains Mono + Space Grotesk (Google Fonts) |
| Icons | Inlined SVG component library |

---

## Project Structure

```
DriveVault/
├── index.html        # The entire application — one file
├── README.md
├── CONTRIBUTING.md
├── SECURITY.md
└── LICENSE
```

---

## License

MIT — see [`LICENSE`](LICENSE).
