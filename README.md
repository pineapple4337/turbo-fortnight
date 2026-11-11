# MAJU Partnerships CRM v0.4

Minimal local-first partnerships and case management for MAJU. **No online database or authentication is connected yet.** Each browser stores its own records. Export JSON regularly. Do not enter confidential partner information into a public demonstration.

## Run locally

Install Node.js 22 or newer, then:

```bash
npm install
npm run dev -- --port 5174
```

Open http://localhost:5174. Import `public/adventure-time-demo.json` or your v0.3 JSON backup to try the application. Import replaces all current browser records after confirmation.

## Publish to GitHub Pages

1. Create a GitHub repository, for example `maju-crm`.
2. Upload **the contents of this folder** to the repository root (including the hidden `.github/workflows/deploy.yml` file). Do not upload `node_modules`, `dist`, or any `.env` file.
3. In the GitHub repository, go to **Settings → Pages → Build and deployment → Source → GitHub Actions**.
4. Push to the `main` branch. The included workflow builds and deploys automatically.
5. Visit `https://YOUR-USERNAME.github.io/REPOSITORY-NAME/`. The Vite configuration automatically uses the repository name for its base path during the GitHub Actions build.

If using a different branch, edit `.github/workflows/deploy.yml`. For a custom domain or user/organization root site, adjust the Vite base setting accordingly.

## Backups and limitations

Data is saved in this browser only. Use **Export** before changing browsers or clearing site data, then **Import** on the other browser. This is a transfer, not real-time collaboration. Never commit actual CRM exports containing personal data into the public GitHub repository.

## Later: shared database

Firestore with Firebase Authentication email/password accounts is a suitable next step. GitHub Pages is public static hosting, so a frontend-only shared password cannot protect Firestore. Enforce access using Firebase Auth and Firestore security rules before adding real partner data. Keep JSON export/import as a backup and migration route.
