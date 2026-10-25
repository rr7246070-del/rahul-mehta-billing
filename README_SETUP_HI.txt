# Easy Billing Online — Setup Guide (Free starter)

This is an installable web app (PWA) for phone/computer. It supports email/password registration and login using Firebase Authentication, and stores bills in Cloud Firestore under each user's account.

## Important: not live until configured and hosted
I cannot create/host a Firebase project under your email from this chat. You must create the free project and paste its web configuration into `index.html`. Firebase free tier limits/terms apply. Hosting providers can also change their free-tier terms.

## 1. Create Firebase project
1. Open https://console.firebase.google.com/ and sign in with your Google account.
2. Click **Create a project** and give it a name such as `easy-billing-rahul`.
3. In Project settings, add a **Web app** (`</>` icon). Copy the Firebase config object.
4. In `index.html`, find `const firebaseConfig = { ... }` and replace all `PASTE_...` values with your actual values. Do not share your config or account password in public posts.

## 2. Turn on email/password login
1. Firebase Console → Build → Authentication → Get started.
2. Sign-in method → **Email/Password** → Enable → Save.

## 3. Create Firestore database and security rules
1. Firebase Console → Build → Firestore Database → Create database.
2. Choose production mode.
3. Open Rules and publish these rules (users can only access their own bills):

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/bills/{billId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}

Do not use public read/write rules. Each user's bills are isolated by their Firebase user ID.

## 4. Host it on the web (HTTPS required)
For installability, use an HTTPS host. Easy option:
- Create a free GitHub account and repository.
- Upload `index.html`, `manifest.json`, and `sw.js` to the repository.
- Repository Settings → Pages → deploy from main branch/root.
- Open the published HTTPS URL. Firebase Authentication → Settings → Authorized domains may need the GitHub Pages domain added.

Other static hosting services can work too. Do not open the file directly as `file://` if you want online login and installability.

## 5. Install as an app
- Android Chrome: open the hosted URL → menu (⋮) → **Install app** or **Add to Home screen**.
- Desktop Chrome/Edge: open the hosted URL → install icon in address bar or menu → Install.
- iPhone: open in Safari → Share → Add to Home Screen.

## Features in this starter
- Email/password register, login and password reset email.
- Item-wise bill rows, quantity, unit, rate and automatic totals.
- Save invoice to cloud database; Saved Bills list and supplier ledger summary.
- Print / Save as PDF.
- Bills are saved per logged-in user.
- No separate manual export/backup is needed for normal cloud storage, but keep records and export/backup strategy as appropriate.

## Limitations to know
- This is a starter web app, not a Play Store APK.
- Online access requires internet and Firebase setup.
- Existing offline HTML app data is not automatically imported into this online account.
- This version summarizes balance from each invoice's initial payment; separate later payments against older dues are not included yet.
- It is not a GST-compliance certification or accounting/tax advice.
