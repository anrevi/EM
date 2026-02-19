# 💰 Paisa — Personal Finance Manager

<div align="center">

![Paisa Banner](https://img.shields.io/badge/Paisa-Personal%20Finance-f5a623?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHRleHQgeT0iMjAiIGZvbnQtc2l6ZT0iMjAiPvCfkqA8L3RleHQ+PC9zdmc+)

[![GitHub Pages](https://img.shields.io/badge/Deployed%20on-GitHub%20Pages-222?style=flat-square&logo=github)](https://YOUR_USERNAME.github.io/paisa)
[![Firebase](https://img.shields.io/badge/Backend-Firebase-FFCA28?style=flat-square&logo=firebase)](https://firebase.google.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

**A beautiful, mobile-first personal expense tracker with real-time Firebase sync and Google Sign-In.**  
No backend server. No monthly fees. Just open and track.

[🚀 Live Demo](https://YOUR_USERNAME.github.io/paisa) · [🐛 Report Bug](../../issues) · [✨ Request Feature](../../issues)

</div>

---

## ✨ Features

| Feature | Details |
|---------|---------|
| 🔐 **Google Auth** | One-tap sign in, persistent session |
| ☁️ **Firebase Sync** | Real-time Firestore, works on all devices |
| 📱 **Mobile-First** | Bottom nav, calculator keypad, swipe-friendly |
| 📊 **Smart Dashboard** | Balance, health score, AI predictions |
| 🌡️ **Expense Heatmap** | GitHub-style 35-day activity grid |
| 🎯 **Budget Tracking** | Per-category limits with overspend alerts |
| 📈 **Analytics** | Daily bars, category donut, 6-month comparison |
| 🏆 **Savings Goals** | Track targets with daily contribution needed |
| 🔄 **Recurring Tracker** | Subscriptions with upcoming due dates |
| 📤 **Export** | Excel (.xlsx), PDF report, CSV |
| 🔒 **Private & Secure** | Only you can see your data — enforced by Firestore rules |
| 🤖 **AI Insights** | Burn rate, lifestyle analysis, budget predictions |

---

## 📸 Screenshots

> Dashboard · Transactions · Budget · Analytics

```
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│  💰 Paisa       │  │  Transactions   │  │  Analytics      │
│                 │  │                 │  │                 │
│  ₹ 42,350       │  │  🍕 Zomato      │  │  [Bar Chart]    │
│  Monthly Bal    │  │  -₹450  UPI     │  │                 │
│                 │  │  🚗 Petrol      │  │  Category Split │
│  [Heatmap]      │  │  -₹280  Cash    │  │  [Donut]        │
└─────────────────┘  └─────────────────┘  └─────────────────┘
```

---

## 🚀 Quick Start

### Prerequisites

- A [Firebase](https://firebase.google.com) account (free)
- A [GitHub](https://github.com) account
- Basic knowledge of copy-paste 😄

---

## 🔥 Firebase Setup (5 minutes)

### Step 1 — Create Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com)
2. Click **"Add project"**
3. Enter project name: `paisa` (or anything)
4. Disable Google Analytics (optional) → **Create project**

### Step 2 — Enable Google Authentication

1. In Firebase Console → **Authentication** → **Sign-in method**
2. Click **Google** → Toggle **Enable** → Save
3. Set your **Project support email**

### Step 3 — Create Firestore Database

1. In Firebase Console → **Firestore Database** → **Create database**
2. Select **"Start in production mode"** → Next
3. Choose your region (e.g., `asia-south1` for India) → Done

### Step 4 — Set Firestore Security Rules

Go to **Firestore → Rules** and replace with:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Each user can only access their own data
    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

Click **Publish**.

### Step 5 — Get Firebase Config

1. Firebase Console → ⚙️ **Project Settings** → **General** tab
2. Scroll to **"Your apps"** → Click **"</>"** (Web app)
3. Register app name (e.g., `paisa-web`) → **Register app**
4. Copy the `firebaseConfig` object — it looks like this:

```javascript
const firebaseConfig = {
  apiKey:            "AIzaSyXXXXXXXXXXXXXXXXX",
  authDomain:        "paisa-xxxxx.firebaseapp.com",
  projectId:         "paisa-xxxxx",
  storageBucket:     "paisa-xxxxx.appspot.com",
  messagingSenderId: "123456789012",
  appId:             "1:123456789012:web:abcdef1234567890"
};
```

### Step 6 — Add Your Config to the App

Open `index.html` and find this section (around line 20):

```javascript
// ↓↓↓ REPLACE THESE VALUES ↓↓↓
const firebaseConfig = {
  apiKey:            "YOUR_API_KEY",
  authDomain:        "YOUR_PROJECT_ID.firebaseapp.com",
  projectId:         "YOUR_PROJECT_ID",
  ...
};
// ↑↑↑ REPLACE THESE VALUES ↑↑↑
```

Replace with your actual config values.

### Step 7 — Add Authorized Domain

1. Firebase Console → **Authentication** → **Settings** → **Authorized domains**
2. Add: `YOUR_USERNAME.github.io`
3. Click **Add**

---

## 🌐 GitHub Pages Deployment

### Option A — Manual Deploy (Easiest)

1. **Fork or create** a new GitHub repository named `paisa`
2. **Upload** `index.html` to the repo root
3. Go to **Settings → Pages**
4. Source: **Deploy from a branch**
5. Branch: `main` → Folder: `/ (root)` → **Save**
6. Wait ~2 minutes → Your app is live at:
   ```
   https://YOUR_USERNAME.github.io/paisa
   ```

### Option B — Auto Deploy with GitHub Actions

Create `.github/workflows/deploy.yml` in your repo:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Every push to `main` will auto-deploy! ✅

---

## 📁 Repository Structure

```
paisa/
├── index.html              ← The entire app (single file)
├── README.md               ← This file
├── LICENSE                 ← MIT License
└── .github/
    └── workflows/
        └── deploy.yml      ← Auto-deploy workflow
```

That's it. Single file. No build tools. No `node_modules`. No dependencies to install.

---

## 🗄️ Firestore Data Structure

```
users/
  {uid}/
    transactions/           ← Collection
      {txnId}/
        type: "expense" | "income"
        amount: 450
        category: "food"
        note: "Zomato dinner"
        method: "UPI"
        date: "2024-01-15T00:00:00.000Z"
        createdAt: Timestamp
        updatedAt: Timestamp

    settings/
      budgets/              ← Document
        total: 30000
        cats: {
          food: 4000,
          transport: 2000,
          ...
        }

    goals/                  ← Collection
      {goalId}/
        name: "Bike Fund"
        icon: "🏍️"
        target: 50000
        saved: 12000
        createdAt: Timestamp
```

---

## 💻 Local Development

No build process needed. Just open the file:

```bash
# Clone your repo
git clone https://github.com/YOUR_USERNAME/paisa.git
cd paisa

# Option 1: Open directly
open index.html

# Option 2: Use a local server (recommended — avoids CORS issues)
npx serve .
# or
python3 -m http.server 8080
# then visit http://localhost:8080
```

> ⚠️ **Important:** Firebase Auth requires a real domain or localhost. Opening `file://` directly may block Google Sign-In. Use a local server.

---

## 🛠️ Customization

### Change Currency

Search for `₹` in `index.html` and replace with your currency symbol (e.g., `$`, `€`, `£`, `৳`).

### Add/Edit Categories

Find the `CATEGORIES` array in the `<script>` section:

```javascript
const CATEGORIES = [
  { id: 'food', name: 'Food', icon: '🍕', color: '#e74c3c' },
  // Add your own:
  { id: 'groceries', name: 'Groceries', icon: '🛒', color: '#27ae60' },
];
```

### Change Default Budget

Find `{ total: 30000, cats: {} }` and change `30000` to your monthly budget.

### Edit Recurring Items

Find `let recurringItems = [...]` and modify the array.

---

## 🔒 Security

- **Auth rules** ensure users can only read/write their own data
- **No admin access** — not even the developer can see your data
- **Google OAuth** — no passwords stored anywhere
- **Offline-first** — data cached in browser IndexedDB
- **HTTPS only** — GitHub Pages enforces HTTPS

---

## 📊 Tech Stack

| Technology | Usage |
|-----------|-------|
| HTML/CSS/JS | App UI — no framework |
| [Firebase Auth](https://firebase.google.com/docs/auth) | Google Sign-In |
| [Cloud Firestore](https://firebase.google.com/docs/firestore) | Real-time database |
| [Chart.js](https://www.chartjs.org/) | Analytics charts |
| [SheetJS (xlsx)](https://sheetjs.com/) | Excel export |
| [jsPDF](https://github.com/parallax/jsPDF) | PDF export |
| [Google Fonts](https://fonts.google.com/) | Sora + DM Mono |
| GitHub Pages | Static hosting |

**Total bundle size:** ~1 file, ~0 build steps, ~0 servers

---

## 🐛 Troubleshooting

### "Sign-in blocked" or "auth/unauthorized-domain"
→ Add your domain to Firebase Console → Authentication → Settings → Authorized domains

### Data not syncing
→ Check your Firestore security rules — make sure they're published  
→ Check browser console for Firebase errors

### Charts not showing
→ Make sure you have transactions added for the current month

### "Failed to get document" error
→ Firestore may not be created yet — follow Step 3 in Firebase Setup

### App shows blank on GitHub Pages
→ Make sure you replaced all `YOUR_API_KEY` placeholders in `index.html`

---

## 📋 Roadmap

- [ ] PWA support (installable on home screen)
- [ ] Push notifications for budget alerts
- [ ] CSV import
- [ ] Multi-currency support
- [ ] Family/shared wallet mode
- [ ] Dark/light theme toggle
- [ ] Bill photo attachment (Firebase Storage)
- [ ] OCR receipt scanning

---

## 🤝 Contributing

Contributions are welcome!

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---

## 🙏 Acknowledgments

- [Firebase](https://firebase.google.com) for the free tier that makes this possible
- [Chart.js](https://chartjs.org) for beautiful charts
- [Google Fonts](https://fonts.google.com) for Sora and DM Mono

---

<div align="center">
Made with ❤️ · <a href="https://YOUR_USERNAME.github.io/paisa">Live App</a> · <a href="../../issues">Report Bug</a>
</div>
