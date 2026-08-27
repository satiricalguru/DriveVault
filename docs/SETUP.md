# 🛠️ Google OAuth 2.0 Setup Guide for DriveVault

DriveVault connects directly from your browser to Google Drive's APIs using OAuth 2.0 Client-Side (Implicit / Token Client) authorization. No client secrets or backend servers are involved.

Follow this step-by-step walkthrough to generate your **Google OAuth 2.0 Client ID**.

---

## 1. Create a Google Cloud Project

1. Open the [Google Cloud Console](https://console.cloud.google.com/).
2. Click the project dropdown in the top navigation bar and select **New Project**.
3. Name your project (e.g., `DriveVault-Auditor`) and click **Create**.
4. Make sure your newly created project is selected in the top bar.

---

## 2. Enable Required Google APIs

DriveVault needs access to the **Google Drive API** and the **Drive Activity API**:

1. In the Cloud Console sidebar, go to **APIs & Services** → **Library**.
2. Search for **Google Drive API**, click on it, and click **Enable**.
3. Return to the Library, search for **Drive Activity API**, click on it, and click **Enable**.

---

## 3. Configure the OAuth Consent Screen

1. Go to **APIs & Services** → **OAuth consent screen**.
2. Select **External** (or **Internal** if using Google Workspace within an organization) and click **Create**.
3. Fill in the required fields:
   - **App name**: `DriveVault`
   - **User support email**: Your email address
   - **Developer contact information**: Your email address
4. Click **Save and Continue**.
5. Under **Scopes**, click **Add or Remove Scopes** and add the following:
   - `https://www.googleapis.com/auth/drive.metadata.readonly`
   - `https://www.googleapis.com/auth/drive.readonly`
   - `https://www.googleapis.com/auth/drive.activity.readonly`
   - `https://www.googleapis.com/auth/drive.appdata`
   - `https://www.googleapis.com/auth/userinfo.profile`
   - `https://www.googleapis.com/auth/userinfo.email`
6. Click **Update** → **Save and Continue**.
7. Under **Test users**, click **Add Users** and add your own Google email address (required while the app is in Testing mode).
8. Click **Save and Continue** → **Back to Dashboard**.

---

## 4. Create OAuth 2.0 Credentials

1. Go to **APIs & Services** → **Credentials**.
2. Click **+ Create Credentials** at the top and select **OAuth client ID**.
3. Under **Application type**, select **Web application**.
4. Set **Name** to `DriveVault Web Client`.
5. Under **Authorized JavaScript origins**, click **+ Add URI** and enter the URLs you will serve DriveVault from:
   - For local development: `http://localhost:5173`
   - For alternative local ports: `http://localhost:8000`, `http://localhost:3000`, `http://127.0.0.1:5173`
   - For GitHub Pages: `https://<your-username>.github.io`
6. Leave **Authorized redirect URIs** blank (DriveVault uses the Google Identity Services popup flow).
7. Click **Create**.
8. A modal will appear displaying your **Client ID** (format: `xxxxxxxxxxxx-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.apps.googleusercontent.com`). Copy this ID.

---

## 5. Configure DriveVault

You can configure DriveVault in either of two ways:

### Option A: Using the In-App Settings (Recommended)
1. Serve DriveVault:
   ```bash
   python3 -m http.server 5173
   ```
2. Open [http://localhost:5173](http://localhost:5173) in your browser.
3. Click **Configuration Setup** (or the gear icon).
4. Paste your **Google OAuth Client ID** and click **Save Configuration**.
5. Your Client ID is securely saved to your browser's `localStorage` and will persist across sessions.

### Option B: Hardcoding into `index.html`
Open `index.html` in an editor and replace `YOUR_GOOGLE_CLIENT_ID`:
```javascript
window.DRIVEVAULT_CONFIG = {
  googleClientId: "YOUR_CLIENT_ID_HERE.apps.googleusercontent.com",
  anthropicProxyUrl: "",
};
```

---

## 6. (Optional) Claude AI Forensics Proxy

If you wish to use the **AI Analysis** tab powered by Claude:
1. Set up an authenticated HTTP proxy endpoint that accepts POST requests with `{ model, prompt, system, stream }` and relays them to Anthropic with your server-side API key.
2. In DriveVault's **Configuration Setup**, enter your proxy endpoint URL into the **Anthropic Proxy URL** field.
3. If left blank, DriveVault seamlessly falls back to offline, local forensic diagnostics heuristics.

---

## 7. Security Notes

- **Zero backend**: Tokens never leave your local browser session and are kept in ephemeral JavaScript memory.
- **Revoking access**: You can revoke DriveVault's permissions at any time via [Google Account Permissions](https://myaccount.google.com/permissions).
