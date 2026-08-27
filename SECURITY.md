# Security Policy

## Supported Versions

DriveVault is a single-file static web application. The current version in `main` is the only supported version.

## What DriveVault Does With Your Data

DriveVault is designed with privacy as a first principle:

- **No backend.** There is no server. Nothing is stored outside your browser.
- **Token in memory only.** The Google OAuth access token is stored in JavaScript runtime memory and is never written to `localStorage`, `sessionStorage`, cookies, or any persistent storage. It is gone when you close or refresh the tab.
- **Direct API calls.** All requests go from your browser directly to Google's APIs (`googleapis.com`, `oauth2.googleapis.com`, `driveactivity.googleapis.com`). No data is routed through any intermediary.
- **Claude analysis is opt-in and proxy-based.** The Analysis feature only activates if you configure your own proxy URL. Even then, only the selected file's content (up to 8,000 characters) is sent — no Drive credentials or tokens.

## Reporting a Vulnerability

If you discover a security vulnerability in DriveVault, please report it responsibly:

1. **Do not open a public GitHub issue** for security vulnerabilities.
2. Use GitHub's private security advisory feature:
   - Go to the **Security** tab of this repository
   - Click **Report a vulnerability**
3. Include:
   - A description of the vulnerability
   - Steps to reproduce it
   - The potential impact
   - Any suggested fix (optional but appreciated)

You can expect an acknowledgement within 48 hours and a fix or public disclosure within 14 days, depending on severity.

## Scope

The following are in scope:

- OAuth token handling (leakage, improper storage, exposure in logs)
- Content Security Policy weaknesses that allow data exfiltration
- XSS vulnerabilities in the file content viewer or hex dump
- Any code path that sends Drive file content to an unintended destination

The following are out of scope:

- Google's own API security (report those to Google)
- Anthropic's API security (report those to Anthropic)
- Attacks that require physical access to the user's machine

## Third-Party Libraries

DriveVault loads the following libraries from CDN. Pin versions before deploying to production and review their own security advisories:

| Library | CDN | Version pinned |
|---|---|---|
| React | unpkg.com | 18 (UMD) |
| Babel standalone | unpkg.com | latest |
| Tailwind CSS | cdn.tailwindcss.com | latest |
| Monaco Editor | cdn.jsdelivr.net | 0.49.0 |
| Chart.js | cdn.jsdelivr.net | latest |
| JSZip | cdnjs.cloudflare.com | 3.10.1 |
| Google GSI | accounts.google.com | N/A (Google-managed) |
