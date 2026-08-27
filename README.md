<p align="center">
  <a href="https://satiricalguru.github.io/DriveVault/">
    <img src="assets/logo.svg" alt="DriveVault Logo" width="108" height="108" />
  </a>
</p>

<h1 align="center">🛡️ DriveVault</h1>

<p align="center">
  <b>Connected App Data Extractor & Forensic Privacy Console for Google Drive</b>
</p>

<p align="center">
  <a href="https://satiricalguru.github.io/DriveVault/"><img src="https://img.shields.io/badge/🚀_Live_Demo-satiricalguru.github.io/DriveVault-06b6d4?style=for-the-badge&logoColor=white" alt="Live Demo" /></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square" alt="License: MIT" />
  <img src="https://img.shields.io/badge/Architecture-100%25_Client--Side-10b981.svg?style=flat-square" alt="100% Client-Side" />
  <img src="https://img.shields.io/badge/Google_Drive_API-v3-4285F4?style=flat-square&logo=googledrive&logoColor=white" alt="Google Drive API v3" />
  <img src="https://img.shields.io/badge/Drive_Activity-v2-34A853?style=flat-square&logo=google&logoColor=white" alt="Drive Activity API" />
  <img src="https://img.shields.io/badge/Privacy-Zero_Backend-8b5cf6?style=flat-square" alt="Zero Backend" />
</p>

---

## ⚡ What is DriveVault?

Third-party apps connected to your Google account routinely write files to your Drive — sync states, diagnostic dumps, database backups, cached tokens, and settings — without any clear visibility in standard Google Drive. 

**DriveVault** is a **single-file, zero-backend client-side forensics engine** that connects directly to Google Drive via OAuth 2.0. It catalogs hidden app-written files, inspects metadata and payloads, flags security identifiers, monitors live modifications, and packages evidence files — all wrapped in an ultra-sleek, cyberpunk glassmorphic dashboard.

---

## 📸 Forensic Dashboard Preview

<p align="center">
  <img src="https://github.com/user-attachments/assets/798d3d6e-bc9f-4abc-87a6-9018970afd8d" alt="DriveVault Dashboard Screenshot" width="100%" />
</p>

---

## 🌟 Key Features

| Feature | Description |
|---|---|
| 🔍 **Connected Apps Auditor** | Clusters and attributes files by their writing application, complete with permission flags, data footprint, last modified date, and automated risk scoring. |
| 📁 **File Explorer & Filtering** | Tree-structured cluster explorer with real-time in-cluster search and instant MIME type filter tags (`JSON`, `TXT`, `IMG`, `BIN`). |
| 🧪 **Payload Inspector** | Inspect file properties, raw JSON metadata, and embedded Monaco Editor views. Binary files feature a windowed hex dump viewer. |
| 🛡️ **Vulnerability Diagnostics** | Automatically scans metadata and text payloads against sensitive keywords (passwords, tokens, sessions, private keys, device IDs, coordinates). |
| 🤖 **AI Forensics & Local Heuristics** | Claude-powered forensic analysis explaining payload purpose, writing app, and privacy concerns (falls back to an offline heuristic analyzer if proxy is unset). |
| 📊 **Interactive Analytics** | Real-time visual metrics: storage by app, file counts, MIME type breakdown, top largest payloads, and modification timelines. |
| 📦 **Bulk Evidence Exports** | Export full ZIP packages (files + per-file JSON metadata), spreadsheet-ready CSV catalogs, or combined JSON forensics reports. |
| 🔴 **Live Diagnostics Watcher** | Real-time background watcher polling Drive for live creations and modifications every 30 seconds with instant delta badges. |
| 🔒 **Zero-Backend Privacy** | Scoped OAuth tokens exist strictly in active JavaScript heap memory. Nothing is ever written to disks, cookies, or remote databases. |

---

## 🔬 Honest Scope — Capabilities & Sandboxing

Google enforces strict app-level isolation across its Drive infrastructure:

| Capability | Supported | Technical Mechanism |
|---|:---:|---|
| **Files saved to your main Drive** | ✅ | Scanned via `drive.readonly` + metadata attributes |
| **App permission signals & history** | ✅ | Introspected via `drive.activity.readonly` & token introspection |
| **DriveVault's own `appDataFolder`** | ✅ | Isolated read/write via `drive.appdata` |
| **Other apps' private `appDataFolder`** | ❌ | **Hard-sandboxed by Google** — no API or workaround exists |

> ℹ️ *DriveVault operates with full transparency: Google prevents any third-party app from reading another application's private `appDataFolder`. DriveVault audits all public files, permission records, and activity signals.*

---

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/satiricalguru/DriveVault.git
cd DriveVault
```

### 2. Start a Local Server

Because DriveVault is a self-contained, zero-build web app, serve it with any static web server:

```bash
# Python 3
python3 -m http.server 5173

# Or with Node.js npx
npx serve -l 5173 .
```

### 3. Configure Google OAuth Client ID

1. Visit **[http://localhost:5173](http://localhost:5173)** in your browser.
2. Click **Configuration Setup** (or the gear icon).
3. Paste your Google OAuth 2.0 Client ID and click **Save Configuration**. Credentials are saved in your browser's `localStorage`.
4. Click **Connect Google Account** to begin scanning!

👉 *For full step-by-step instructions on creating a Google Cloud OAuth Client ID and enabling Drive APIs, see the [`docs/SETUP.md`](docs/SETUP.md) guide.*

---

## 🔐 Required OAuth Scopes

| Scope | Purpose |
|---|---|
| `drive.metadata.readonly` | Read metadata of all files (including app-associated items) |
| `drive.readonly` | Download and inspect file content locally |
| `drive.activity.readonly` | Surface automated application activity and actors |
| `drive.appdata` | Manage DriveVault's own isolated test storage |
| `userinfo.profile` | Display user display name and profile picture |
| `userinfo.email` | Display connected account address |

---

## 🛠️ Tech Stack

- **Runtime:** Single-file static web application (zero bundler required)
- **UI Framework:** React 18 (Babel standalone)
- **Styling:** Tailwind CSS (CDN) + Custom Glassmorphism Theme
- **Editor & Code Viewer:** Monaco Editor (`vs-dark`)
- **Visual Analytics:** Chart.js
- **Archiving:** JSZip
- **Auth:** Google Identity Services (GSI) Token Client Flow
- **Typography:** Space Grotesk + JetBrains Mono

---

## 📂 Project Structure

```
DriveVault/
├── index.html        # Complete standalone web application
├── assets/
│   └── logo.svg      # DriveVault branding logo
├── docs/
│   └── SETUP.md      # Detailed Google Cloud OAuth setup guide
├── CONTRIBUTING.md   # Guidelines for contributing
├── SECURITY.md       # Responsible security disclosure policy
├── LICENSE           # MIT License
└── README.md
```

---

## 🤝 Contributing

Contributions are welcome! Please see [`CONTRIBUTING.md`](CONTRIBUTING.md) for guidelines. Remember: DriveVault is strictly a **no-backend, zero-build single-file** architecture.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
