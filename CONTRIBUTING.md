# Contributing to DriveVault

Thanks for your interest in improving DriveVault. Contributions are welcome — here's everything you need to know.

---

## Ground Rules

- Be respectful and constructive in all communication.
- This project is a **privacy and transparency tool**. Any contribution that weakens user honesty (e.g. overpromising capabilities, hiding what the app does) will not be accepted.
- Keep the **no-backend, no-build** constraint intact. The entire app must remain a single `index.html` that works with `python3 -m http.server`.

---

## Ways to Contribute

### 🐛 Bug Reports
Open an issue with:
- What you did
- What you expected
- What actually happened
- Browser + OS

### 💡 Feature Requests
Open an issue describing:
- The use case you're trying to solve
- Why it fits the project's scope (Drive / privacy audit)

### 🔧 Pull Requests
1. Fork the repository
2. Create a branch: `git checkout -b feature/your-feature-name`
3. Make your changes in `index.html` (and any `docs/` files if relevant)
4. Test manually in at least one browser (Chrome and Firefox preferred)
5. Open a pull request with a clear description of what changed and why

---

## Development Setup

No build tooling required.

```bash
git clone https://github.com/YOUR_USERNAME/DriveVault.git
cd DriveVault

# Add your Google OAuth Client ID to index.html
# Then serve:
python3 -m http.server 5173
```

Open [http://localhost:5173](http://localhost:5173).

---

## Code Style

- Keep everything inside the single `index.html` file.
- Use functional React components and hooks only — no class components.
- Tailwind utility classes for all styling. Add custom values via `tailwind.config` in the `<script>` block if needed.
- All API calls go through `fetchWithBackoff`. Don't introduce raw `fetch` calls.
- New features that make Google API calls must respect the concurrency limiter (`createLimiter(5)`).
- Keep comments in plain English. The `inferAppName`, `scanMetadata`, and `scoreRisk` functions especially benefit from explanation when modified.

---

## Things We Will Not Accept

- Anything that claims to read another app's private `appDataFolder` — that's sandboxed by Google and this project is honest about it.
- Features that send user data to any third-party server not already disclosed (Google, Anthropic proxy).
- Adding a build step or bundler dependency — keep it zero-config.
- Breaking the privacy mode toggle.

---

## Questions?

Open an issue and tag it `question`.
