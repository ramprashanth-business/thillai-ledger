# Thillai Decors & Events — Cash Ledger (Live Website)

A mobile-first cash ledger web app with cloud backup. Every entry is saved to
a real database (Firebase Firestore) tied to your login, so it's safe even if
you lose your phone or switch devices.

## What's in this folder
- `index.html` — the whole app (single file, no build step needed)
- `manifest.json`, `icon-192.png`, `icon-512.png` — lets you "Install" it like an app
- `sw.js` — makes it load fast / work briefly offline

---

## Step 1 — Create your free Firebase project (5 min)

1. Go to https://console.firebase.google.com and sign in with any Google account.
2. Click **Add project** → name it (e.g. `thillai-ledger`) → you can turn off Google
   Analytics for this → **Create project**.
3. In the left sidebar, click **Build → Authentication → Get started**.
   - Under **Sign-in method**, enable **Email/Password** → Save.
4. In the left sidebar, click **Build → Firestore Database → Create database**.
   - Choose **Start in production mode** → pick a location close to India (e.g. `asia-south1`) → Enable.
5. Still in Firestore, go to the **Rules** tab and replace the contents with:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /users/{userId}/entries/{entryId} {
         allow read, write: if request.auth != null && request.auth.uid == userId;
       }
     }
   }
   ```
   Click **Publish**. This makes sure only you (once signed in) can read or write your own entries.

6. Click the **gear icon (⚙) → Project settings**, scroll to **Your apps**, click the
   **</> (Web)** icon, give it any nickname, click **Register app**.
7. Firebase will show you a `firebaseConfig` object like this:

   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "thillai-ledger.firebaseapp.com",
     projectId: "thillai-ledger",
     storageBucket: "thillai-ledger.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```
   Copy this whole block.

8. Open `index.html` in any text editor (or directly on GitHub — see Step 2), find this
   near the top of the last `<script type="module">` section:

   ```js
   const firebaseConfig = {
     apiKey: "YOUR_API_KEY",
     ...
   };
   ```
   Replace it with the block you copied from Firebase. Save the file.

---

## Step 2 — Publish it on GitHub Pages (5 min)

1. Go to https://github.com and click **New repository**.
   - Name it anything, e.g. `thillai-ledger`.
   - Set it to **Public** (GitHub Pages on the free tier needs a public repo).
   - Click **Create repository**.
2. On the new repo page, click **uploading an existing file**.
3. Drag in all the files from this folder: `index.html`, `manifest.json`,
   `icon-192.png`, `icon-512.png`, `sw.js`.
   - If you haven't edited `firebaseConfig` yet, you can also do it here: after
     uploading, click on `index.html` in your repo → the pencil (✏️) icon to edit →
     paste in your config → **Commit changes**.
4. Click **Commit changes** to finish the upload.
5. Go to the repo's **Settings** tab → **Pages** (left sidebar).
6. Under **Build and deployment → Source**, choose **Deploy from a branch**.
   Under **Branch**, choose `main` and folder `/ (root)` → **Save**.
7. Wait about a minute, then refresh that Pages settings screen — it will show:
   **"Your site is live at `https://YOUR-USERNAME.github.io/thillai-ledger/`"**
8. Open that link on your phone. Tap **Create an account**, set an email + password
   (this is just for your app, not a real email account) — you're in.

To install it like an app: open the link in Chrome (Android) or Safari (iPhone) →
**Add to Home Screen**. It'll open full-screen with your app icon, no browser bar.

---

## Updating the app later
Whenever I (or you) make changes to `index.html`, just re-upload the file to the
same GitHub repo (or edit it directly on github.com) and commit — GitHub Pages
updates automatically within a minute.

## Notes
- Real bank/UPI transaction auto-detection isn't possible for a personal app —
  that requires a licensed payment aggregator agreement. The GPay/PhonePe/Paytm
  buttons in the app open those apps with the amount and note pre-filled so you
  can pay, then you confirm the entry in the ledger yourself.
- Firebase's free tier ("Spark plan") comfortably covers personal use like this
  at no cost.
