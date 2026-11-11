<<<<<<< HEAD
# MAJU Partnerships CRM v0.5

React/Vite CRM with partners, cases, updates, calendar, briefs, JSON import/export and optional Firebase shared storage.

## Local test mode

`npm install` then `npm run dev -- --port 5174`. Without Firebase environment variables, this uses browser localStorage. Data is not shared between browsers.

## Shared Firebase setup

1. Create a Firebase project at https://console.firebase.google.com and add a **Web app**. Copy its Firebase configuration values.
2. Under **Build > Firestore Database**, create a database (production mode). Open **Rules**, paste the supplied `firestore.rules`, and **Publish**. Do not use test-mode rules for real data.
3. Under **Authentication > Sign-in method**, enable **Email/Password**. Disable public sign-up in your own UI (this app has no sign-up button). In **Authentication > Users**, create each teammate's email/password account. Firebase Console may require you to use the Authentication users interface or Admin SDK for account creation.
4. Copy `.env.example` to `.env.local`, fill in the four `VITE_FIREBASE_*` fields using the Firebase Web app config. These are Firebase client configuration values, **not** service-account credentials. Never put admin keys in frontend variables.
5. Each approved account needs a Firestore document at `members/{firebase-auth-user-uid}` with the boolean field `active: true`. Copy the UID from **Authentication > Users**. Create the `members` collection and documents manually in the Firestore console. Without this document, login works but database access is denied.
6. Restart Vite, sign in, then import the sample JSON to seed the **shared** database. Import replaces the shared dataset; export a backup first.

## GitHub Pages

1. Upload all project files, including `.github/workflows/deploy.yml`, to your repository's `main` branch.
2. Under repository **Settings > Pages**, select **GitHub Actions**. Do **not** choose Jekyll or Static HTML templates.
3. Under **Settings > Secrets and variables > Actions > Variables**, create these repository **variables** (not secrets): `VITE_FIREBASE_API_KEY`, `VITE_FIREBASE_AUTH_DOMAIN`, `VITE_FIREBASE_PROJECT_ID`, `VITE_FIREBASE_APP_ID`. Paste the corresponding Firebase web app config values. These values are public by design; Firestore rules provide the security.
4. Under Firebase Authentication > Settings > Authorized domains, add `<username>.github.io` (domain only, not the path).
5. Push to `main` or run **Actions > Deploy GitHub Pages > Run workflow**. Check the Actions log for errors.

The Vite base URL is calculated from the GitHub repository name in the workflow. GitHub Pages serves the frontend; Firestore stores shared records.

## Important limitations

- This version uses Firestore realtime listeners and individual documents for partners, cases and engagements. Discussion updates are currently embedded in partner documents. Simultaneous edits to the **same partner** can overwrite each other; avoid concurrent edits to one partner until updates are moved into separate documents.
- The 'Updated by' name is still entered by the user, and is not cryptographically tied to the authenticated account. This is **not** a tamper-proof audit log.
- Any approved member can edit or delete all records and import/replace shared data. Keep JSON backups and restrict who is approved.
- Do not enable public anonymous access or publish permissive Firestore rules.
- Test with fictional data before entering real partner contacts or sensitive notes.
=======
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
>>>>>>> dd9644bc6ee210e8c9a70d329c7c7359532edf61
