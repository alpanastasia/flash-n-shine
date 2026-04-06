# Flash & Shine — Setup Guide

**Without Firebase:** open `index.html` directly in any browser. Works immediately in local mode (data stays in that browser only).

**With Firebase:** follow the steps below to enable email/password login and cross-device sync. Takes ~10 minutes. Free on Firebase's Spark plan.

---

## Step 1 — Create a Firebase project

1. Go to https://console.firebase.google.com
2. Click **Add project** → name it (e.g. "flash-and-shine") → Create
3. Disable Google Analytics (not needed)

---

## Step 2 — Enable Email/Password Sign-In

1. Go to **Build → Authentication → Get started**
2. Click **Email/Password** under Sign-in providers
3. Toggle **Enable** → Save

---

## Step 3 — Create your user account

1. In Authentication, go to the **Users** tab
2. Click **Add user**
3. Enter your email and a strong password → Add user
4. Repeat for any other users (up to a few — this app is personal-scale)

---

## Step 4 — Create a Firestore database

1. Go to **Build → Firestore Database → Create database**
2. Choose **Start in production mode** → pick a region → Done
3. Go to the **Rules** tab, replace everything with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

4. Click **Publish**

---

## Step 5 — Get your Firebase config

1. Go to ⚙️ **Project Settings** → scroll to **Your apps** → click **Add app** → Web (</>)
2. Give it a nickname → **Register app** (no need to enable Firebase Hosting)
3. Copy the `firebaseConfig` object:

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123:web:abc"
};
```

---

## Step 6 — Paste config into index.html

Open `index.html` in any text editor. Find near the top of the `<script>` section:

```js
const FIREBASE_CONFIG = {
  apiKey: "",
  authDomain: "",
  ...
};
```

Replace the empty strings with your values. Save the file.

---

## Step 7 — Add authorized domains

1. Firebase Console → **Authentication → Settings → Authorized domains**
2. Add `localhost` for local use
3. If hosting online (e.g. GitHub Pages), add that domain too

---

## Done!

Open `index.html` → sign in with the email/password you created in Step 3. Your cards sync automatically. Open on your phone and sign in with the same credentials — same decks, same progress.

---

## Hosting on GitHub Pages (optional, for phone access without file transfer)

1. Create a GitHub account → new **public** repository → upload `index.html`
2. Go to repo **Settings → Pages → Source: main branch** → Save
3. Your app is live at `https://yourusername.github.io/reponame`
4. Add that URL to Firebase Authorized Domains (Step 7)
5. Open on your phone, sign in — done

---

## Keyboard shortcuts (Review screen)

| Key   | Action      |
|-------|-------------|
| Space | Flip card   |
| 1     | Fail        |
| 2     | Hard        |
| 3     | Good        |
| 4     | Easy        |
| H     | Toggle hint |

---

## CSV import format

```
Foreign,Translation,Tags,Example_Foreign,Example_Translation
slovo,слово,noun;neuter,"Toto je slovo.","Это слово."
los,жребий|лось,noun,"Los padl na mě.",""
```

- Multiple meanings: separate with `|` in the Translation column
- Tags: semicolon-separated
- Example columns are optional
- First row is a header (skipped)
- UTF-8 encoding
