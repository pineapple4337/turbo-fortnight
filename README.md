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
