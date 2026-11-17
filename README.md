# CareerReady

Career readiness calculator (50 careers) with a password-protected admin panel.
Admin changes are saved on the server and shown to every visitor.

```
public/index.html            the website
netlify/functions/login.js   POST /api/login    -> checks username/password, returns a 2-hour token
netlify/functions/careers.js GET /api/careers   -> public list
                             PUT /api/careers   -> save list (admin token required)
                             DELETE /api/careers-> reset to defaults (admin token required)
```

## 1. Set the admin login (one time)

The username and password are **not in the code**. Set three environment variables in Netlify
(Site configuration -> Environment variables):

| Name | What to put |
|------|-------------|
| `ADMIN_USER` | your admin username |
| `ADMIN_PASSWORD` | a long, unique password |
| `ADMIN_SECRET` | any long random text (at least 16 characters). Create one with `openssl rand -hex 32` |

If any of these is missing, admin login is switched off (it never falls back to a default password).
Change `ADMIN_SECRET` at any time to log every admin out.

## 2. Deploy

Functions need a real deploy (not just uploading `index.html`). Easiest options:

**Netlify CLI**
```bash
npm install
npm install -g netlify-cli
netlify login
netlify deploy --prod --build
```

**Or Git:** push this folder to GitHub and import it in Netlify. Publish directory and functions
folder are already set in `netlify.toml`.

## Run locally

```bash
npm install
cp .env.example .env     # then edit the three values
npx netlify dev          # http://localhost:8888
npm test                 # backend tests
```

## Notes
- Careers saved by the admin are stored in Netlify Blobs. "Reset to default careers" in the admin
  panel deletes the saved list and everyone goes back to the 50 built-in careers.
- Assessment history stays in each visitor's own browser (private). The admin "stats" cards
  therefore show data from the admin's own device only.
