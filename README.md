# PixelRate — Photo Rating System
> DSA Group Project · Group 3 · Daffodil International University

A cloud-synced photo rating web application built with vanilla JavaScript and Firebase Realtime Database. Users rate photos across 5 datasets and all data syncs in real time. The admin panel provides full visibility into ratings, rankings, and dataset management.

**Live at:** `https://tahmadim080.github.io/photo-rating-system/`

---

## Features

- Register and login with hashed password security
- Rate photos 1–5 stars across 5 member datasets (20 slots × 5 photos = 100 per dataset)
- Auto-advances to next photo on star click — no extra button needed
- Seeded shuffle — same photo order every time you log back in
- Real-time sync to Firebase after every rating
- Admin panel with live stats, ranked photo grid, and CSV export
- Admin can upload photos slot-by-slot per dataset and delete individual photos
- Reset all user data with one button (photos stay intact)
- About Us modal with pentagon team layout
- Profile page with change password feature
- Fully free to host — GitHub Pages + Firebase free tier

---

## Project Structure

```
your-repo/
├── index.html      ← Main app (login, dashboard, rating, results)
├── admin.html      ← Admin panel (stats, photo manager, CSV export)
└── README.md       ← This file
```

---

## Setup Guide

### Step 1 — Create Firebase Project

1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Click **Add project** → name it `pixelrate` → Create
3. In the left sidebar click **Build** → **Realtime Database**
4. Click **Create Database** → choose any region → select **Start in test mode** → Enable
5. Copy the database URL shown at the top — it looks like:
   ```
   https://pixelrate-xxxxx-default-rtdb.firebasedatabase.app
   ```

### Step 2 — Paste Firebase URL into both files

Open `index.html` and find:
```javascript
const FIREBASE_URL = 'PASTE_YOUR_DATABASE_URL_HERE';
```
Replace with your copied URL. Do the same in `admin.html`.

Also change the admin password in `admin.html`:
```javascript
const ADMIN_PASSWORD = 'pixelrate2024'; // ← change this
```

### Step 3 — Deploy to GitHub Pages

1. Create a new **public** repository on GitHub
2. Upload `index.html`, `admin.html`, and `README.md`
3. Go to **Settings** → **Pages** → Source: **main branch / root** → Save
4. Wait 2–3 minutes → your site is live

**Main app:** `https://USERNAME.github.io/REPO/`
**Admin panel:** `https://USERNAME.github.io/REPO/admin.html`

---

## How to Upload Photos (Admin)

1. Open `admin.html` and log in
2. Click **Manage Photos** in the navbar
3. Select a dataset tab (Tanvir, Alvi, Provat, Mim, Nadim)
4. You will see 20 slots — each slot holds exactly 5 photos
5. Click **+ Add Photos** on any slot to upload images
6. Photos are saved directly to Firebase and appear on the main site immediately
7. Hover any uploaded photo and click **✕** to delete it

If no photos are uploaded for a slot, placeholder images are shown automatically.

---

## How to Reset All User Data (Admin)

Click **Reset All Data** in the admin navbar. You will be asked to confirm twice and type `RESET` before anything is deleted. This wipes all user accounts and ratings. Photos are not affected.

---

## CSV Export

Click **Export Ranked CSV** in the admin panel. The file contains two sections:

**Section 1 — All individual ratings:**

| User Name | User Email | Dataset | Slot | Photo Filename | Stars Given |
|-----------|-----------|---------|------|----------------|-------------|

**Section 2 — Photo rankings highest to lowest:**

| Rank | Dataset | Slot | Photo Filename | Average Rating | Total Ratings |
|------|---------|------|----------------|----------------|---------------|

---

## DSA Concepts Used

| Concept | Where Used |
|---------|-----------|
| Hash Map | User accounts and ratings stored as key-value objects |
| Polynomial Rolling Hash | Password hashing — irreversible hex string |
| Fisher-Yates Shuffle | Random photo order per user in O(n) time |
| Seeded PRNG (LCG) | Same shuffle every login for the same user |
| Merge by Timestamp | Sync conflict resolution — latest rating wins |

---

## Tech Stack

| Technology | Purpose |
|-----------|---------|
| HTML5 / CSS3 | Structure and Everforest dark theme |
| Vanilla JavaScript | All logic — no frameworks |
| Firebase Realtime Database | Cloud sync for all users |
| localStorage | Offline-first local cache |
| GitHub Pages | Free static hosting |
| Google Fonts (Roboto) | Typography |

---

## Group Members

| Name | Student ID |
|------|-----------|
| Fahim | 252-15-607 |
| Nadim | 252-15-267 |
| Mim | 252-15-609 |
| Provat | 252-15-303 |
| Alvi | 252-15-282 |

---

## Firebase Rules (Keep Database Open)

If Firebase test mode expires after 30 days, go to **Realtime Database → Rules** and set:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

---

*PixelRate · Group 3 · Daffodil International University · DSA Project*
