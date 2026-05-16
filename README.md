# 🛡️ DriveVault — Connected App Data Extractor

> Audit every app that touches your Google Drive. Inspect files, review permissions, detect sensitive data, and export evidence packages — all client-side, no backend required.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Status](https://img.shields.io/badge/status-active-brightgreen)
![No Backend](https://img.shields.io/badge/backend-none-green)
![Google Drive API](https://img.shields.io/badge/API-Google%20Drive%20v3-4285F4?logo=googledrive)

---

## What is DriveVault?

Third-party apps connected to your Google account quietly write files to your Drive — sync states, backups, configs, tokens — without any visibility in the standard Google Drive UI. DriveVault surfaces all of it.

DriveVault is a **single-file, zero-backend web app** that connects to your Google account via OAuth 2.0 and lets you:

- See every file that connected apps have written to your Drive
- Inspect raw file content (JSON viewer, hex dump, image preview)
- Audit which apps hold Drive permissions and what they've been doing
- Detect files containing sensitive fields (tokens, emails, location, device IDs)
- Export everything as a ZIP, CSV, or full JSON report
- Monitor live changes (30-second poll via Drive Changes API)

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
Groups all Drive files by the app that created or modified them. Each app card shows file count, storage used, last activity, permission level, and a risk score based on what sensitive metadata signals were found.

### 📁 File Explorer
Browse all app-attributed files with a sidebar tree grouped by inferred app name. Select any file to open the inspector.

### 🧪 File Inspector (3 tabs)
- **Metadata** — File ID, MIME type, size, timestamps, `appProperties`, capabilities, full raw JSON
- **Content** — Smart viewer: Monaco Editor for JSON/text, inline preview for images, hex dump for binary files. Detects sensitive field names automatically.
- **Analysis** — Claude-powered forensic explanation of what the file is, what app wrote it, and privacy risks (requires proxy configuration; falls back to local heuristics)

### 📊 Analytics Dashboard
Donut chart of storage by app, bar chart of files per app, MIME type breakdown, largest files list, modification timeline.

### 📦 Bulk Export
- **Download ZIP** — all selected files + per-file metadata JSON + manifest
- **Metadata CSV** — spreadsheet-ready file list
- **Full JSON Report** — metadata + base64 content for text/JSON files

### 🔴 Live Watcher
Polls Drive every 30 seconds. Displays a live changes badge and a changes log when files are created or modified.

### 🔒 Privacy Mode
Disable all Claude API calls. Everything stays in the browser tab.

---

## Screenshots

> _Add screenshots here after running the app. Suggested captures:_
> - Landing page with scope transparency panel
> - Connected Apps tab showing app cards with risk badges
> - File Inspector > Content tab showing Monaco JSON viewer
> - Analytics dashboard with charts

---

## Quick Start

### 1. Get a Google OAuth Client ID

See the full walkthrough in [`docs/SETUP.md`](docs/SETUP.md).

Short version:
1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a project → enable **Google Drive API** and **Drive Activity API**
3. OAuth consent screen → add the six scopes listed below
4. **Credentials** → Create **OAuth 2.0 Client ID** → Web Application
5. Add `http://localhost:5173` (or your domain) to **Authorized JavaScript Origins**
6. Copy the Client ID

### 2. Configure the App

Open `index.html` and find this line near the top:

```javascript
window.DRIVEVAULT_CONFIG = {
  googleClientId: "YOUR_GOOGLE_CLIENT_ID",
  anthropicProxyUrl: "",
};
```

Replace `YOUR_GOOGLE_CLIENT_ID` with your OAuth Client ID.

### 3. Serve and Open

DriveVault is a static single-file app. It must be served over HTTP (not opened as `file://`) because the Google OAuth library requires an origin.

```bash
# Python (no install needed)
python3 -m http.server 5173

# Node.js
npx serve . -p 5173

# VS Code
# Use the "Live Server" extension, then open index.html
```

Then visit [http://localhost:5173](http://localhost:5173).

### 4. Connect Your Google Account

Click **Connect Google Account**, sign in, and approve the requested scopes. DriveVault will begin scanning your Drive immediately.

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

## Optional: Claude Analysis

The **Analysis** tab in the File Inspector can explain what any file contains, which app likely wrote it, and flag privacy risks. This feature calls the Anthropic API, but browsers cannot hold a secret API key safely.

You have two options:

**Option A — Deploy a simple proxy** (recommended)  
Create a tiny server that accepts `{ model, system, prompt, stream }` and forwards to `https://api.anthropic.com/v1/messages` with your secret key. Set `anthropicProxyUrl` in `DRIVEVAULT_CONFIG` to your proxy URL.

**Option B — Skip it**  
Leave `anthropicProxyUrl` empty. The Analysis tab falls back to a local heuristic scan (no API call, fully offline).

---

## Security & Privacy

- **No backend.** DriveVault is 100% client-side JavaScript.
- **Token in memory only.** The Google access token is never written to `localStorage`, `sessionStorage`, or cookies. It lives only in the JS runtime and is gone when you close the tab.
- **No data leaves your browser** except for requests directly to Google's APIs (and Anthropic's API if analysis is configured).
- **Disconnect at any time.** Click Disconnect and the token is cleared. You can also revoke DriveVault's Google permission at [myaccount.google.com/permissions](https://myaccount.google.com/permissions).

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | React 18 (Babel standalone, no build step) |
| Styling | Tailwind CSS (CDN) |
| Auth | Google Identity Services (GSI) token client flow |
| Drive API | Google Drive API v3 (raw `fetch`) |
| Activity API | Google Drive Activity API v2 |
| Code Viewer | Monaco Editor (CDN) |
| Charts | Chart.js (CDN) |
| Export | JSZip (CDN) |
| Fonts | JetBrains Mono + Space Grotesk (Google Fonts) |
| AI Analysis | Anthropic API via user-configured proxy |

---

## Project Structure

```
DriveVault/
├── index.html        # The entire application — one file
├── docs/
│   └── SETUP.md      # Detailed Google Cloud Console setup guide
├── README.md
├── CONTRIBUTING.md
├── SECURITY.md
└── LICENSE
```

---

## Contributing

Pull requests are welcome. See [`CONTRIBUTING.md`](CONTRIBUTING.md) for guidelines.

---

## License

MIT — see [`LICENSE`](LICENSE).

---

## Acknowledgements

- [Google Drive API v3 docs](https://developers.google.com/workspace/drive/api/reference/rest/v3)
- [Google Drive Activity API docs](https://developers.google.com/drive/activity)
- [Google Identity Services](https://developers.google.com/identity/oauth2/web/guides/overview)
- [Anthropic Claude API](https://docs.anthropic.com)
