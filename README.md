# Family Health Record

A shared, real-time family health tracker (illnesses, appointments, medications,
vaccines) with trend charts. Data is stored in Firebase Firestore so every
family member sees the same, live-updating record. Hosted for free on GitHub
Pages.

There are three files:
- `index.html` — the whole app (no build step needed)
- `firestore.rules` — security rules to paste into Firebase
- `README.md` — this file

## 1. Create a Firebase project (5 minutes, free)

1. Go to [console.firebase.google.com](https://console.firebase.google.com) and sign in with any Google account.
2. Click **Add project**, give it any name (e.g. "family-health"), and finish the wizard. You can disable Google Analytics for this project — it isn't needed.
3. Once the project is created, click the **Web** icon (`</>`) on the project overview page to register a new web app. Give it any nickname and click **Register app**. You do *not* need Firebase Hosting for this step.
4. Firebase will show you a `firebaseConfig` object that looks like this:
   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "family-health-xxxxx.firebaseapp.com",
     projectId: "family-health-xxxxx",
     storageBucket: "family-health-xxxxx.appspot.com",
     messagingSenderId: "1234567890",
     appId: "1:1234567890:web:abcdef123456"
   };
   ```
   Copy this whole object.

## 2. Enable Firestore

1. In the left sidebar of the Firebase console, go to **Build → Firestore Database**.
2. Click **Create database**. Choose any nearby region and start in **production mode**.
3. Once it's created, go to the **Rules** tab, delete the default contents, and paste in everything from `firestore.rules` in this folder. Click **Publish**.

This app doesn't use accounts or passwords — instead, each household gets a
random 8-character **family code** that acts like a shared secret. Only
someone who has that code (because you shared it with them) can read or
write that household's data. The rules above just check that a code was
provided; they don't validate a specific list of codes, since a static
site has nowhere private to store one. This is reasonable protection for a
private family tool, but it is not bank-grade security — anyone who
obtained your family code could read or edit your data. Don't post the
code publicly, and don't use this for anything beyond casual household
record-keeping.

## 3. Paste your config into the app

1. Open `index.html` in a text editor.
2. Find this block near the top of the `<script type="module">` section:
   ```js
   const firebaseConfig = {
     apiKey: "REPLACE_ME",
     authDomain: "REPLACE_ME.firebaseapp.com",
     projectId: "REPLACE_ME",
     storageBucket: "REPLACE_ME.appspot.com",
     messagingSenderId: "REPLACE_ME",
     appId: "REPLACE_ME"
   };
   ```
3. Replace it with the real config object you copied in step 1. Save the file.

(It's normal and expected for this config to be visible in your public
source code — Firebase's web config is not a secret by itself. Your
Firestore **rules** are what actually control access, which is why step 2
matters.)

## 4. Push to GitHub

1. Create a new repository on GitHub (public or private — Pages works with
   both if you have GitHub Pro/Team/Enterprise; free accounts need a
   **public** repo for Pages).
2. Add the three files (`index.html`, `firestore.rules`, `README.md`) to the
   repository, either by uploading them in the GitHub web UI or via git:
   ```bash
   git init
   git add index.html firestore.rules README.md
   git commit -m "Family health record app"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO.git
   git push -u origin main
   ```

## 5. Turn on GitHub Pages

1. In your repository on GitHub, go to **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to "Deploy from a branch".
3. Set **Branch** to `main` and the folder to `/ (root)`. Click **Save**.
4. Wait a minute, then refresh the page — GitHub will show your live URL,
   something like `https://YOUR-USERNAME.github.io/YOUR-REPO/`.

## 6. Start using it

1. Open your GitHub Pages URL.
2. Click **Start a new family record**, fill in your household name and the
   people you're tracking. You'll be shown a family code (e.g. `7F3KQ9XM`).
3. Send your site URL and that family code to the rest of your household
   (text, email, whatever you'd use to share a password).
4. On their own device, they open the same URL and choose **Join an
   existing family**, entering the code you gave them.

Everyone who joins with the same code sees the same timeline, charts, and
reminders, and changes sync automatically without needing to refresh.

## Troubleshooting

- **"Couldn't connect" on first load** — double check the `firebaseConfig`
  values in `index.html` match exactly what Firebase gave you, and that you
  published the rules in step 2.
- **"We couldn't find a family with that code"** — codes are case-sensitive
  and 8 characters; check for typos, especially between `0`/`O` and `1`/`I`
  (the generator avoids these, but double-check when typing).
- **Changes made on one device don't show on another** — make sure both
  devices joined with the exact same family code, and that neither is
  offline.
