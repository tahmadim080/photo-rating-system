# PixelRate — Photo Rating System
### DSA Group Project

A full-stack photo rating web application built as a **single HTML file**. No backend, no server, no installation needed. Runs entirely in the browser using `localStorage` as the database.

---

## Table of Contents

- [Live Demo Setup](#live-demo-setup)
- [How It Works](#how-it-works)
- [Project Structure](#project-structure)
- [Adding Your Photos](#adding-your-photos)
- [Changing Names & Celebrities](#changing-names--celebrities)
- [Login & Account System](#login--account-system)
- [DSA Concepts Used](#dsa-concepts-used)
- [Deploying to GitHub Pages](#deploying-to-github-pages)
- [Group Member Responsibilities](#group-member-responsibilities)
- [FAQ](#faq)

---

## Live Demo Setup

To run the project locally, just **double-click** `index.html` — it opens in your browser.

To run it online for everyone, deploy to **GitHub Pages** (free). See [Deploying to GitHub Pages](#deploying-to-github-pages).

---

## How It Works

```
User registers / logs in
        ↓
Picks a dataset (one per group member)
        ↓
Rates photos 1–5 stars (100 photos per dataset)
        ↓
All ratings saved to browser localStorage
        ↓
Results page shows all rated photos + Export CSV
```

- **No internet needed** to run (except for loading photos from Picsum)
- **Data is saved per browser** — each device/browser has its own data
- **Multiple users** can register on the same device
- **Resume anytime** — your progress is saved automatically

---

## Project Structure

```
your-repo/
│
├── index.html              ← THE ENTIRE APP (only file you need)
│
└── photos/                 ← YOUR PHOTO FOLDER (when using local photos)
    ├── tanvir/
    │   ├── celebrity_name/
    │   │   ├── celebrity_name_01.jpg
    │   │   ├── celebrity_name_02.jpg
    │   │   ├── celebrity_name_03.jpg
    │   │   ├── celebrity_name_04.jpg
    │   │   └── celebrity_name_05.jpg
    │   └── ... (20 celebrity folders)
    ├── alvi/
    ├── provat/
    ├── mim/
    └── nadim/
```

> **Note:** The current version uses online photos from [Lorem Picsum](https://picsum.photos) for the showcase. If you want to use real celebrity photos, follow the [Adding Your Photos](#adding-your-photos) section.

---

## Adding Your Photos

### Step 1 — Name your photo files

Each celebrity needs **exactly 5 photos**, named like this:

```
ronaldo_01.jpg
ronaldo_02.jpg
ronaldo_03.jpg
ronaldo_04.jpg
ronaldo_05.jpg
```

The name must be **lowercase**, **no spaces** (use underscores). This name must **exactly match** the `id` field in the code.

### Step 2 — Create the folder structure

Inside the `photos/` folder, create subfolders like this:

```
photos/
└── tanvir/              ← member folder (must match id in code)
    └── ronaldo/         ← celebrity folder (must match id in code)
        ├── ronaldo_01.jpg
        ├── ronaldo_02.jpg
        ├── ronaldo_03.jpg
        ├── ronaldo_04.jpg
        └── ronaldo_05.jpg
```

### Step 3 — Update the code

Open `index.html`, find this comment near the top of the `<script>` section:

```
⚠️  CHANGE NAMES HERE — THIS IS THE ONLY PLACE YOU NEED TO EDIT
```

Change the `id` and `name` for each celebrity:

```javascript
// BEFORE:
{ id: 'cele1', name: 'Celebrity 1' },

// AFTER:
{ id: 'ronaldo', name: 'Ronaldo' },
```

The `id` = folder name + file prefix (must be lowercase, no spaces)  
The `name` = display name shown on screen (can be anything)

---

## Changing Names & Celebrities

### Where exactly to edit

Open `index.html` in any text editor (Notepad, VS Code, etc.) and search for:

```
CHANGE NAMES HERE
```

You will see 5 blocks like this — one per group member:

```javascript
{
  id: 'tanvir',         // ← folder name on GitHub (keep lowercase, no spaces)
  member: 'Tanvir',     // ← display name shown on the website
  color: '#a7c080',     // ← card accent color (you can change this)
  celebrities: [
    { id: 'cele1',  name: 'Celebrity 1'  },   // ← change these
    { id: 'cele2',  name: 'Celebrity 2'  },   // ← change these
    // ... 20 total
  ]
},
```

**Rules:**
| Field | Rule | Example |
|-------|------|---------|
| `id` (dataset) | lowercase, no spaces | `'tanvir'` |
| `member` | any display name | `'Tanvir'` |
| `id` (celebrity) | lowercase, no spaces, matches folder & filename | `'ronaldo'` |
| `name` (celebrity) | any display name | `'Ronaldo'` |

### Dataset colors (optional)

Each member has a color for their card. You can change it:

```javascript
color: '#a7c080',   // Tanvir  — green
color: '#7fbbb3',   // Alvi    — teal
color: '#d699b6',   // Provat  — purple
color: '#dbbc7f',   // Mim     — gold
color: '#e69875',   // Nadim   — orange
```

Use any hex color code you like.

---

## Login & Account System

### How accounts work

- Users **register** with name, email, and password
- Passwords are **hashed** using a polynomial rolling hash (a DSA concept!)
- All accounts stored in `localStorage` under the key `pr_users`
- Session stored under `pr_sess` — stays logged in until you click Logout
- Each user's ratings stored separately under `pr_p_EMAIL`

### Data storage keys

| Key | What it stores |
|-----|----------------|
| `pr_users` | All registered accounts (email, name, hashed password) |
| `pr_sess` | Currently logged-in user's email |
| `pr_p_tanvir@email.com` | That user's ratings & progress for all datasets |

### Important notes

- **Data is stored per browser** — registering on Chrome does not carry over to Firefox
- **Clearing browser data** (cookies/cache) will delete all accounts and ratings
- **Different devices** = different data (this is expected for a DSA project demo)
- There is **no admin account** — everyone registers as a normal user

### How to view raw data (for demo/presentation)

1. Open the site in browser
2. Press `F12` → go to **Application** tab
3. Click **Local Storage** → click the site URL
4. You can see all raw JSON data stored — great to show the teacher!

---

## DSA Concepts Used

| Concept | Where used |
|---------|-----------|
| **Hash Map** | User storage (`pr_users` object), ratings storage |
| **Hashing** | Password hashing via polynomial rolling hash |
| **Fisher-Yates Shuffle** | Randomizing photo order per user |
| **Seeded PRNG** | Same photo order every login for same user (reproducible shuffle) |
| **Array** | Photo list, dataset structure, rating order |

### Password hashing function (polynomial rolling hash)

```javascript
hash(s) {
  let h = 0;
  for (let i = 0; i < s.length; i++)
    h = Math.imul(31, h) + s.charCodeAt(i) | 0;
  return h.toString(16);
}
```

### Seeded Fisher-Yates shuffle

```javascript
shuffle(arr, seed) {
  const a = [...arr];
  let s = Math.abs(seed) || 9999;
  for (let i = a.length - 1; i > 0; i--) {
    s = (s * 1664525 + 1013904223) & 0xffffffff;
    const j = Math.abs(s) % (i + 1);
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}
```

---

## Deploying to GitHub Pages

### First time setup

1. Go to [github.com](https://github.com) and sign in
2. Click **+** → **New repository**
3. Name it anything (e.g. `pixelrate`)
4. Set it to **Public**
5. Do NOT add README or .gitignore
6. Click **Create repository**
7. On the next page, click **uploading an existing file**
8. Drag and drop `index.html` (and the `photos/` folder if using local photos)
9. Click **Commit changes**
10. Go to **Settings** → **Pages**
11. Under Source, select **main** branch → **/ (root)** → **Save**
12. Wait 2–3 minutes
13. Your site is live at: `https://YOUR-USERNAME.github.io/REPO-NAME/`

### Updating the site

1. Make changes to `index.html` locally
2. Go to your repo on GitHub
3. Click on `index.html` → click the **pencil icon** (Edit)
4. Paste your new code → **Commit changes**
5. Changes go live in ~1 minute

### Adding photos to GitHub

1. In your repo, click **Add file** → **Upload files**
2. You cannot upload whole folders directly — create the folder path by naming files like: `photos/tanvir/ronaldo/ronaldo_01.jpg` in the file name field
3. Or use [GitHub Desktop](https://desktop.github.com/) to drag and drop entire folder structures

---

## Group Member Responsibilities

| Member | Dataset ID | Folder | Their job |
|--------|-----------|--------|-----------|
| Tanvir | `tanvir` | `photos/tanvir/` | Collect 20 celebrities × 5 photos, rename files correctly |
| Alvi | `alvi` | `photos/alvi/` | Collect 20 celebrities × 5 photos, rename files correctly |
| Provat | `provat` | `photos/provat/` | Collect 20 celebrities × 5 photos, rename files correctly |
| Mim | `mim` | `photos/mim/` | Collect 20 celebrities × 5 photos, rename files correctly |
| Nadim | `nadim` | `photos/nadim/` | Collect 20 celebrities × 5 photos, rename files correctly |

### Photo checklist for each member

- [ ] Chose 20 celebrities
- [ ] Collected 5 photos per celebrity (100 photos total)
- [ ] Renamed all photos to `celebid_01.jpg` format
- [ ] Created correct folder structure `photos/memberid/celebid/`
- [ ] Updated `id` and `name` in `index.html` for all 20 celebrities
- [ ] Uploaded photos to GitHub repo

---

## FAQ

**Q: The photos aren't loading**  
A: Make sure the folder names and file names exactly match the `id` fields in the code. Everything must be lowercase with underscores, no spaces.

**Q: I cleared my browser and lost all data**  
A: Yes — data lives in `localStorage`. Always export your CSV before clearing browser data. Go to Results page → **Export CSV**.

**Q: Can two people share ratings across devices?**  
A: No — each browser has its own localStorage. This is by design for the project demo. Everyone rates independently.

**Q: How do I reset all data for a fresh demo?**  
A: Press `F12` → Application → Local Storage → right-click the site → **Clear**. Or open browser Settings → Clear browsing data → Cookies and site data.

**Q: The site shows a blank page / error**  
A: Open `F12` → Console tab and check the error message. Most likely a typo in a celebrity `id` or a missing quote in the code.

**Q: How do I change the site title?**  
A: Search for `<title>PixelRate` in `index.html` and change it.

**Q: Can I add more than 20 celebrities?**  
A: Yes — add more `{ id: '...', name: '...' }` entries to the celebrities array and add the corresponding photos. The progress bar automatically adjusts (it counts total photos = celebrities × 5).

---

## Tech Stack

| Technology | Purpose |
|-----------|---------|
| HTML5 | Structure |
| CSS3 | Styling (Everforest dark theme) |
| Vanilla JavaScript | All logic, no frameworks |
| localStorage | Database |
| Google Fonts (Roboto) | Typography |
| Lorem Picsum | Placeholder photos (showcase mode) |

---

*Built for DSA course project. Single-file architecture — everything in `index.html`.*
