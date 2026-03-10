# PixelRate — Photo Rating System
> DSA Group Project | HTML + CSS + JavaScript + Firebase Realtime Database

---

## What is this?

PixelRate is a photo rating web app. Users register, log in, and rate photos 1 to 5 stars across 5 datasets (one per group member). All ratings sync to Firebase so the admin can see everyone's data in real time from any device.

No server needed. No installation. Just two HTML files on GitHub Pages.

---

## Files in this repo

```
your-repo/
├── index.html      ← Main app (login, rating, results)
├── admin.html      ← Admin dashboard (see all users and ratings)
├── README.md       ← This file
└── photos/         ← Your own photos (optional)
    ├── tanvir/
    ├── alvi/
    ├── provat/
    ├── mim/
    └── nadim/
```

---

## Setup Guide — From Zero to Live

### Step 1 — Create a Firebase project

1. Go to https://console.firebase.google.com
2. Sign in with your Google account
3. Click **Add project**
4. Enter project name: `pixelrate` → click **Continue**
5. Disable Google Analytics (not needed) → click **Create project**
6. Wait for it to create → click **Continue**

### Step 2 — Create a Realtime Database

1. In your Firebase project, look at the left sidebar
2. Click **Build** → **Realtime Database**
3. Click **Create Database**
4. Choose any location (closest to you) → click **Next**
5. Select **Start in test mode** → click **Enable**
6. You will see your database URL at the top — it looks like:
   ```
   https://pixelrate-abc12-default-rtdb.firebaseio.com
   ```
7. Copy this URL — you will need it in the next step

### Step 3 — Paste the URL into both files

Open `index.html` in any text editor (Notepad, VS Code) and find this line:

```javascript
const FIREBASE_URL = 'PASTE_YOUR_DATABASE_URL_HERE';
```

Replace it with your actual URL:

```javascript
const FIREBASE_URL = 'https://pixelrate-abc12-default-rtdb.firebaseio.com';
```

Do the exact same thing in `admin.html`.

Also in `admin.html`, change the admin password:

```javascript
const ADMIN_PASSWORD = 'pixelrate2024';   // change this to something secret
```

Save both files.

### Step 4 — Create a GitHub repository

1. Go to https://github.com and sign in (or create a free account)
2. Click the **+** icon (top right) → **New repository**
3. Repository name: `pixelrate` (or anything you like)
4. Set visibility to **Public** (required for free GitHub Pages)
5. Leave all checkboxes unchecked
6. Click **Create repository**

### Step 5 — Upload your files

1. On your new empty repo page, click **uploading an existing file**
2. Drag and drop these three files:
   - `index.html`
   - `admin.html`
   - `README.md`
3. Scroll down and click **Commit changes**

### Step 6 — Enable GitHub Pages

1. In your repo, click **Settings** (top menu)
2. In the left sidebar, click **Pages**
3. Under Source, select branch **main** and folder **/ (root)**
4. Click **Save**
5. Wait 2 to 3 minutes

Your site is now live at:
```
https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/
```

Admin panel is at:
```
https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/admin.html
```

### Step 7 — Test it

1. Open your live URL
2. Register an account
3. Pick a dataset and rate a few photos
4. Open admin.html URL and enter your admin password
5. Click Refresh — you should see your account and ratings

If everything works, share the main URL with your group and classmates.

---

## How to Update Files Later

**Small changes (editing code):**
1. Go to your repo on GitHub
2. Click on the file (e.g. `index.html`)
3. Click the pencil icon to edit
4. Make changes → click **Commit changes**
5. Wait 1 minute → live

**Uploading photo folders (use GitHub Desktop):**
1. Download GitHub Desktop from https://desktop.github.com
2. Sign in and clone your repo to your computer
3. Add your files to the repo folder on your computer
4. Open GitHub Desktop → it shows your new files
5. Write a commit message → click **Commit to main** → **Push origin**
6. Wait 1 minute → live

---

## Adding Your Own Photos

Currently the site uses random online photos. To use real celebrity photos:

**Naming rules:**
- Filename must be lowercase with underscores, no spaces
- Must match the `id` field in the code exactly
- Each celebrity needs exactly 5 photos numbered `_01` to `_05`

Example for celebrity with id `ronaldo`:
```
ronaldo_01.jpg
ronaldo_02.jpg
ronaldo_03.jpg
ronaldo_04.jpg
ronaldo_05.jpg
```

**Folder structure:**
```
photos/
└── tanvir/
    └── ronaldo/
        ├── ronaldo_01.jpg
        ├── ronaldo_02.jpg
        ├── ronaldo_03.jpg
        ├── ronaldo_04.jpg
        └── ronaldo_05.jpg
```

**Update the code in index.html:**

Find this comment:
```
CHANGE NAMES HERE — THIS IS THE ONLY PLACE YOU NEED TO EDIT
```

Change each celebrity id and name:
```javascript
// Before:
{ id: 'cele1', name: 'Celebrity 1' },

// After:
{ id: 'ronaldo', name: 'Ronaldo' },
```

Then find the `buildPhotoList` function and change the photo path:
```javascript
// Before (online placeholder):
path: 'https://picsum.photos/seed/' + ds.id + '_' + cele.id + '_' + i + '/800/600'

// After (your own photos):
path: 'photos/' + ds.id + '/' + cele.id + '/' + cele.id + '_' + num + '.jpg'
```

---

## Changing Dataset Names and Colors

Find the `const DS = [...]` block in `index.html`:

```javascript
{
  id: 'tanvir',       // folder name — lowercase, no spaces
  member: 'Tanvir',   // display name on the website
  color: '#a7c080',   // card accent color
  celebrities: [ ... ]
},
```

Dataset colors:

| Member | Color   | Name         |
|--------|---------|--------------|
| Tanvir | #a7c080 | Forest Green |
| Alvi   | #7fbbb3 | Teal         |
| Provat | #d699b6 | Purple       |
| Mim    | #dbbc7f | Gold         |
| Nadim  | #e69875 | Orange       |

---

## Admin Panel

Open `admin.html` on your live site. Features:

| Section | What it shows |
|---------|---------------|
| Global Stats | Total users, total ratings, completion % |
| Dataset Overview | Progress bars per member across all users |
| Users Table | Every user and their rating count per dataset |
| All Ratings | Every rated photo with stars, username, date |
| Export All CSV | Full ratings data download |
| Export Users CSV | User progress summary download |

---

## How the Sync Works

```
User rates a photo
        |
        v
Saved to localStorage instantly (works offline)
        |
        v
Pushed to Firebase Realtime Database
        |
        v
Admin panel reads Firebase and sees everyone's data
```

A small spinning indicator appears bottom-right when syncing.

---

## Login and Accounts

- Users register with name, email, and password
- Passwords are hashed using polynomial rolling hash (a DSA concept)
- On login, user data is pulled from Firebase and merged with local cache
- After every rating, data is pushed to Firebase automatically
- Multiple users can use the site from different devices — all data syncs

---

## DSA Concepts Used

| Concept | Where Used |
|---------|-----------|
| Hash Map | User storage object, ratings object |
| Polynomial Rolling Hash | Password hashing |
| Fisher-Yates Shuffle | Randomizing photo order per user |
| Seeded PRNG | Same photo order every login for same user |
| Array | Photo list, dataset structure |
| Merge Algorithm | Sync conflict resolution — latest timestamp wins |

---

## Troubleshooting

**Ratings not saving / syncing**
Check that FIREBASE_URL is correctly pasted in both files. Make sure your Firebase database is in test mode (allows read and write without authentication).

**Admin panel shows Failed to load**
The FIREBASE_URL in admin.html is wrong or missing. Also check the Firebase console — make sure Realtime Database is created and rules allow read access.

**Photos not loading**
Folder names and file names must exactly match the `id` in the code. All lowercase, underscores not spaces.

**Site shows blank page**
Press F12, open the Console tab, read the error message. Usually a quote or syntax issue in the code.

**Firebase test mode warning**
Firebase test mode expires after 30 days. To keep it open, go to Firebase Console → Realtime Database → Rules and set:
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

---

## Group Member Checklist

**One person does once:**
- [ ] Create Firebase project and Realtime Database
- [ ] Copy database URL into index.html and admin.html
- [ ] Change admin password in admin.html
- [ ] Create GitHub repo and upload all files
- [ ] Enable GitHub Pages
- [ ] Test the full flow (register, rate, check admin panel)
- [ ] Share live URL with the group

**Each member does for their dataset:**
- [ ] Choose 20 celebrities
- [ ] Collect 5 photos per celebrity (100 photos total)
- [ ] Rename all photos to the correct format (celebid_01.jpg)
- [ ] Create correct folder structure (photos/memberid/celebid/)
- [ ] Update celebrity names in index.html
- [ ] Upload photos to GitHub repo

---

## Tech Stack

| Technology | Purpose |
|-----------|---------|
| HTML5 / CSS3 | Structure and Everforest dark theme |
| Vanilla JavaScript | All logic, no frameworks |
| localStorage | Local cache for instant offline access |
| Firebase Realtime Database | Cloud database, syncs all users in real time |
| Google Fonts Roboto | Typography |
| Lorem Picsum | Placeholder photos for showcase mode |

---

*PixelRate — DSA Group Project*
