# Punch List — setup guide

A real-time, multi-user punch list app for tracking lingering renovation items. Open the URL, type your first name, you're in. Employees add and check off items, leave notes, and filter by room or assignee. Updates show up instantly on everyone's device. No accounts, no passwords.

**Stack:** static HTML/JS front end (deployable as a GitHub Pages site) + Firebase Firestore for the shared database + Firebase anonymous auth (so random crawlers can't poke at your data, even though there's no sign-up). No server to run, free tier is plenty for a small crew.

---

## What's in this folder

| File | What it is |
| --- | --- |
| `index.html` | The whole app — front end, styles, and Firebase wiring (your config is already filled in) |
| `firestore.rules` | Security rules to paste into the Firebase console |
| `README.md` | This file |

---

## Where you are

Your Firebase config is already pasted into `index.html` — **step 2 is done**. Pick up at step 3 below.

## Remaining setup

### 3. Enable Anonymous Authentication

In the Firebase console (https://console.firebase.google.com → your `rutkowski-punchlist` project):

1. Left sidebar → **Build → Authentication** → **Get started**.
2. Open the **Sign-in method** tab.
3. Click **Anonymous** in the providers list.
4. Toggle **Enable** on. Click **Save**.

### 4. Create the Firestore database

1. Left sidebar → **Build → Firestore Database** → **Create database**.
2. Pick a region close to you (e.g. `us-central` or `nam5`). Click **Next**.
3. Choose **Start in production mode**. Click **Create**.
4. Once it's created, click the **Rules** tab.
5. Replace everything in the rules editor with the contents of `firestore.rules` (in this folder). Click **Publish**.

### 5. Put it on GitHub Pages

1. Go to **https://github.com/new** and create a new repository — public is fine, name it whatever (e.g. `punch-list`).
2. On the new repo page, click **uploading an existing file** and drag in `index.html` (and optionally `firestore.rules` and `README.md`). Commit.
3. In your repo, go to **Settings → Pages**.
4. Under **Source**, choose **Deploy from a branch**, branch **main**, folder **/ (root)**. Click **Save**.
5. Wait about a minute, refresh the Pages screen, and you'll see a URL like `https://yourusername.github.io/punch-list/`. That's your live site.

### 6. Authorize your GitHub Pages domain in Firebase

1. Firebase console → **Authentication → Settings → Authorized domains**.
2. Click **Add domain** and add `yourusername.github.io` (just the username part, no `/punch-list` suffix). Save.

---

## Day-to-day use

**First time:** Open the site, type your first name (or initials), click **Continue**. Add a few rooms via **Manage** in the top right. Start adding items.

**Adding employees:** Send them the GitHub Pages URL. They open it, type their name, hit Continue. Their name shows up in the **Assigned to** dropdown automatically. They can fix typos under **Manage → Your name**.

**Day-to-day actions:** Add an item with title, room, assignee, and notes. Tick the box to mark done — the app records who did it and when. Filter by status (Open / Done / All), room, assignee, or a search term. Sort or group by room or assignee. Edit and delete with the icons on each item.

---

## FAQ

**Is this free?** Yes for any reasonable team size. Firebase's Spark (free) plan covers up to 50K Firestore reads/day, 20K writes/day, and 1 GB storage — far more than a punch list app needs.

**Can someone outside our team get in?** Only if they have the URL. There are no passwords — anyone with the link can use the app. Treat the URL like a shared resource: don't post it publicly.

**Where's the data stored?** In your Firebase project, in Firestore. You own it. You can export it any time from the Firebase console.

**Mobile?** Yes, the layout is mobile-friendly. Employees can use it from their phone browser — they can also add it to their home screen for a one-tap shortcut (Safari: Share → Add to Home Screen; Chrome: ⋮ → Install app).

**Will it remember me on my phone?** Yes — your name is stored in your browser, and Firebase keeps you signed in across visits.

---

## Troubleshooting

**Page is blank / spinner forever.** Open your browser's developer console (F12 → Console tab) and look for the red error. Most likely your domain isn't authorized (step 6).

**"Missing or insufficient permissions"** when loading items. Your `firestore.rules` weren't published. Re-do step 4.5.

**"Anonymous sign-in isn't enabled" when entering a name.** Anonymous auth isn't turned on. Re-do step 3.
