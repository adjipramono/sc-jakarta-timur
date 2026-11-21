# Scooter Jam · Service Admin

A static, single-file admin app (`index.html`) for SC Jakarta Timur transactions.
It has no server: the browser signs in with Google and reads/writes the Google Sheet
directly through the Google Sheets API. Only Google accounts that the sheet is shared
with (as Editor) can read or change data.

Nothing secret is in this repo. The OAuth Client ID is public by design, and the sheet
link is typed in by the admin, not stored in the code.

## One-time setup

### 1. Google Cloud (free)
1. Go to https://console.cloud.google.com and create a project, e.g. `Scooter Jam Admin`.
2. **APIs & Services → Library** → search **Google Sheets API** → **Enable**.
3. **Google Auth Platform** (OAuth consent screen):
   - Branding: app name `Scooter Jam Admin`, your support email.
   - Audience: **External**, publishing status **Testing**. Under **Test users**, add every admin's Gmail.
   - Data access: add the scope `https://www.googleapis.com/auth/spreadsheets`.
4. **Clients → Create client** → type **Web application**.
   - **Authorized JavaScript origins**: `https://<your-github-username>.github.io`
     (add your custom domain too if you use one). No redirect URI is needed.
5. Copy the **Client ID** (ends with `.apps.googleusercontent.com`).

### 2. Put the Client ID in the app
Open `index.html`, find `const CLIENT_ID = 'PASTE_YOUR_CLIENT_ID...'` and paste yours.

### 3. Publish on GitHub Pages (free)
1. Create a repository (public), upload `index.html` (and this README).
2. **Settings → Pages → Build and deployment → Deploy from a branch** → `main` / `/ (root)` → Save.
3. After a minute the app is live at `https://<username>.github.io/<repo>/`.

### 4. Sheet sharing
- Sheet **Share → General access: Restricted**.
- Add each admin as **Editor**. Removing someone from the sheet removes their access to the app.

## Daily use
1. Open the app link, paste the sheet link once (it is remembered on that device), tap **Sign in with Google**.
2. Google may say "Google hasn't verified this app": tap **Continue** (expected for a private app in Testing mode).
3. Sign-in lasts about an hour; when it expires a yellow bar appears. Tap **Sign in again** and repeat the last action. Nothing you typed is lost.
4. On a phone: browser menu → **Add to Home Screen**.

## Notes
- Columns are matched by **header name**. Columns can be moved, but keep header text unchanged.
- Tabs required: Transaction, Service, Customer, Customer Vehicle, Vehicle Type, Machine Type, Mechanic, Service Type.
- New rows are written to the first empty row (by column A) of each tab.
- There is no server-side lock: if two admins save at the exact same moment, the app warns if a transaction ID got duplicated.