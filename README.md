# (Ke)Vault — Personal Finance Tracker

> A mobile-first personal finance web app with Google Sheets sync, AI-powered insights, multi-account tracking, savings goals, and transfer management. Built as a single HTML file — no frameworks, no build tools, no server required.

**Live at:** [kevinsornet.github.io/Kevault](https://kevinsornet.github.io/Kevault)

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Screenshots & Pages](#screenshots--pages)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Google Sheets Integration](#google-sheets-integration)
- [KevAI / Kevinsights Setup](#kevai--kevinsights-setup)
- [Data & Privacy](#data--privacy)
- [Tech Stack](#tech-stack)
- [File Structure](#file-structure)
- [Deployment](#deployment)
- [Troubleshooting](#troubleshooting)

---

## Overview

Vault is a personal budgeting app designed for everyday use on mobile and desktop. It runs entirely in the browser as a single `index.html` file — no backend, no database, no login system. Your data lives in your browser's `localStorage` and optionally syncs to a Google Sheet you own.

It was built for personal use with a focus on:
- Logging expenses, income, and transfers quickly
- Seeing where your money goes across multiple accounts
- Setting and tracking savings goals linked to specific accounts
- Getting AI-generated financial insights through **Kevinsights (KevAI)**
- Keeping a live backup in Google Sheets automatically

---

## Features

### 📊 Dashboard
- **Net worth** calculated live across all linked accounts
- **Monthly income vs. expense** summary cards
- **Transfer fee tracker** — shows total fees paid this month, number of transfers, and total volume moved (appears automatically once you make a transfer)
- **6-month dual-bar chart** showing income (green) and expenses (red) side by side per month
- **Sources summary** — all your accounts with live balances, clickable for detail
- **Recent transactions** list
- **First-time setup banner** prompting Google Sheets connection

### 💸 Transactions
- Log **expenses**, **income**, and **transfers** between accounts
- **21 expense categories** including Food & Dining, Transport, Credit Card Payment, Investment, Phone Plan & Add-ons, Loan Payment, Insurance, and more
- **11 income categories** including Salary, Freelance, Stock/Crypto, Bonus, Refund/Cashback, and more
- **Edit** and **delete** any transaction
- **Transfer between accounts** with optional service/transfer fee — fee is tracked separately and deducted from the source account

### 📈 Analytics (Transactions Tab)
- **Month navigator** — browse any past or future month with ‹ › arrows
- **4 summary cards** — Income, Expenses, Net Saved, Savings Rate (color-coded green/yellow/red)
- **Expense breakdown** — every category with a color bar showing amount and % of total
- **Income breakdown** — same for income sources
- **Daily spending sparkline** — 31-bar chart showing spending per day with today highlighted
- **Activity heatmap calendar** — full month grid color-coded by spending intensity
  - Purple = expense-dominant days, green = income-dominant days
  - Today highlighted with amber ring
  - Tap any day to see a full breakdown: income, spent, transfer, net, and every transaction for that day
- **Kevinsights (KevAI)** — AI-generated analysis of your spending (requires Cloudflare Worker setup — see below)

### 🎯 Goals
- Create **savings goals** with a name, emoji, target amount, starting amount, optional deadline, and optional monthly contribution target
- **Link a goal to a source account** — when you add funds to a goal, it automatically logs an expense from the linked account and deducts it from that account's balance
- **Progress bar** with % complete, amount saved, and amount remaining
- **Add Funds** button to top up any goal
- Linked account and monthly target displayed as badges on the goal card

### 💳 Sources (Accounts)
- Add multiple accounts: Bank, E-Wallet, Cash, Credit Card, Investment
- Each source has a **custom name**, **type**, **starting balance**, and **color**
- **Edit** any source's name, type, balance, or color
- **Delete** sources
- Balances are **calculated live** — starting balance + all income - all expenses - transfer fees + transfers in - transfers out
- **Click any source** to open a detailed view:
  - Balance breakdown (Total In, Total Out, Fees Paid)
  - 6-month in vs. out sparkline
  - **Linked goals section** — balance allocation bar showing how much of the current balance is allocated to each goal vs. unallocated, plus per-goal contribution history
  - Full transaction history grouped by month, filterable by All / Out / In

### 📗 Google Sheets Sync
- **Create a new sheet** directly from the app — sets up Transactions, Goals, and Sources tabs with styled headers automatically
- **Browse your Drive** — search and select any existing spreadsheet without leaving the app
- **Auto-sync on every change** — add/edit/delete a transaction, goal, or source, and the sheet updates instantly
- **Startup sync** — on every page load, the app re-authenticates and syncs automatically:
  - Local has data, sheet is empty → uploads to sheet
  - Sheet has data, local is empty → downloads from sheet
  - Both have data → quietly pushes local to sheet (local is source of truth)
- **5-minute interval sync** — background sync every 5 minutes during active sessions
- **Page-close sync** — final push when you close or refresh the tab
- **Safety guards:**
  - Upload is blocked if local data is empty (prevents accidentally wiping the sheet)
  - Download shows exact row counts before overwriting local data
  - Empty sheet triggers a warning before any download
- **Sync status dot** in the header — grey (no sheet), purple (linked), amber (syncing), green (synced), red (error)
- Manual **Upload Local → Sheet** and **Download Sheet → Local** buttons available at any time

### ✦ Kevinsights (KevAI)
- AI-powered financial insights using **Claude** (Anthropic)
- Analyzes your spending for the selected month and returns:
  - **What you're doing well** 🌟 — praises good savings habits and positive trends
  - **Watch out** ⚠️ — flags concerns like overspending or high fees
  - **Tips to save more** 💡 — actionable suggestions based on your actual categories
- Uses real numbers from your data — mentions specific amounts, categories, and goal progress
- Requires a **Cloudflare Worker** as a proxy (see setup below)

---

## Screenshots & Pages

| Page | Description |
|---|---|
| **Home** | Net worth, monthly stats, transfer fee tracker, 6-month chart, sources, recent transactions |
| **Transactions** | Month analytics, heatmap calendar, day detail, Kevinsights, transaction list with filters |
| **Goals** | Savings goals with progress, linked accounts, monthly targets |
| **Settings** | Google Sheets management, currency selector, data controls |

---

## Getting Started

### Prerequisites
- A modern browser (Chrome, Safari, Firefox, Edge)
- A Google account (for Sheets sync)
- Optional: Anthropic API key + Cloudflare account (for KevAI)

### Quick Start
1. Open [kevinsornet.github.io/Kevault](https://kevinsornet.github.io/Kevault) in your browser
2. Tap **💳** in the header to add your first account (source)
3. Tap **+** in the bottom nav to log your first transaction
4. Tap **📊** to connect Google Sheets for automatic backup

---

## Configuration

Two constants at the top of the `<script>` block in `index.html` control integrations:

```js
// Your Google OAuth Client ID (for Sheets sync)
const GOOGLE_CLIENT_ID = 'your-client-id.apps.googleusercontent.com';

// Your Cloudflare Worker URL (for KevAI)
const KEVAI_PROXY_URL = ''; // e.g. 'https://kevai-proxy.yourname.workers.dev'
```

---

## Google Sheets Integration

Vault uses the **Google Sheets API** and **Google Drive API** via OAuth 2.0 (browser-based, no secrets in code).

### One-time Google Cloud Setup

1. Go to [console.cloud.google.com](https://console.cloud.google.com) → create a new project (e.g. `Kevault`)
2. **APIs & Services → Library** → enable **Google Sheets API** and **Google Drive API**
3. **APIs & Services → OAuth consent screen** → External → fill in app name and your email → save
4. Add yourself as a **test user** (and anyone else you want to share the app with)
5. **APIs & Services → Credentials → Create Credentials → OAuth 2.0 Client ID**
   - Application type: **Web application**
   - Authorized JavaScript origins: `https://yourusername.github.io`
   - Authorized redirect URIs: `https://yourusername.github.io/Kevault/`
6. Copy the **Client ID** and paste it as `GOOGLE_CLIENT_ID` in `index.html`

### Sharing with others
- Anyone who wants to use the app must be added as a **test user** in the OAuth consent screen
- All users sign in with their own Google account — their data stays in their own Drive
- The Client ID is safe to be public — it only works from the authorized domain

### Sheet structure
When a new sheet is created from Vault, it contains three tabs:

| Tab | Columns |
|---|---|
| **Transactions** | ID, Date, Type, Description, Category, Amount, Source ID, Source Name |
| **Goals** | ID, Name, Emoji, Target, Saved, Remaining, % Complete, Deadline, Linked Source, Monthly Target |
| **Sources** | ID, Name, Type, Starting Balance, Color |

---

## KevAI / Kevinsights Setup

KevAI uses the **Anthropic Claude API** to analyze your finances. Because browser-based apps can't call the Anthropic API directly (CORS restriction), a tiny **Cloudflare Worker** acts as a proxy.

### Step 1 — Get an Anthropic API Key
1. Go to [console.anthropic.com](https://console.anthropic.com) → sign up
2. **API Keys → Create Key** → name it `Kevault`
3. **Copy the full key immediately** — it starts with `sk-ant-api03-` and is shown only once
4. Go to **Billing** → add payment and buy at least **$5 of credits**
   - Cost per analysis: ~$0.006 (less than a cent)
   - $5 lasts approximately 800 analyses

### Step 2 — Deploy Cloudflare Worker
1. Go to [workers.cloudflare.com](https://workers.cloudflare.com) → sign up free
2. **Workers & Pages → Create → Create Worker** → name it `kevai-proxy`
3. Click **Deploy** then **Edit Code**
4. Replace all code with:

```js
export default {
  async fetch(req, env) {
    if (req.method === 'OPTIONS') return new Response(null, { headers: cors() });
    const body = await req.json();
    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': env.ANTHROPIC_KEY,
        'anthropic-version': '2023-06-01'
      },
      body: JSON.stringify(body)
    });
    const data = await res.json();
    return Response.json(data, { headers: cors() });
  }
};
function cors() {
  return {
    'Access-Control-Allow-Origin': '*',
    'Access-Control-Allow-Methods': 'POST, OPTIONS',
    'Access-Control-Allow-Headers': 'Content-Type'
  };
}
```

5. Click **Deploy**
6. **Settings → Variables and Secrets → Add**
   - Type: **Secret**
   - Name: `ANTHROPIC_KEY`
   - Value: your `sk-ant-api03-...` key
   - Click **Deploy**
7. Copy your Worker URL (e.g. `https://kevai-proxy.yourname.workers.dev`)

### Step 3 — Add the URL to Vault
In `index.html`, find and update:
```js
const KEVAI_PROXY_URL = 'https://kevai-proxy.yourname.workers.dev';
```
Then commit and push to GitHub.

---

## Data & Privacy

- **All data is stored locally** in your browser's `localStorage` — nothing is sent to any server except when syncing to your own Google Sheet
- **Google Sheets** data lives in your personal Google Drive — Vault only has access to files it creates or that you explicitly select
- **KevAI** sends your monthly financial summary (totals and categories — no transaction IDs or personal details) to your own Cloudflare Worker, which forwards it to Anthropic. Anthropic's data handling is governed by their [privacy policy](https://www.anthropic.com/privacy)
- **The Google Client ID** in the source code is intentionally public — it cannot be used to access any data without the user actively signing in through Google's OAuth popup
- **No ads, no tracking, no analytics** — the app makes no network calls except to Google APIs and your own Cloudflare Worker

### Clearing your data
Go to **Settings → Clear All Data** to wipe all local transactions, goals, and sources. This does not affect your Google Sheet.

---

## Tech Stack

| Component | Technology |
|---|---|
| **App** | Vanilla HTML, CSS, JavaScript — single file, no frameworks |
| **Fonts** | Syne (headings), DM Mono (numbers) via Google Fonts |
| **Authentication** | Google Identity Services (OAuth 2.0, implicit flow) |
| **Sheets Sync** | Google Sheets API v4, Google Drive API v3 |
| **AI Insights** | Anthropic Claude (claude-sonnet-4) via Cloudflare Worker proxy |
| **Hosting** | GitHub Pages (static) |
| **Storage** | Browser localStorage |

---

## File Structure

```
Kevault/
├── index.html      ← The entire app (HTML + CSS + JS in one file)
└── README.md       ← This file
```

That's it. No `node_modules`, no `package.json`, no build step.

---

## Deployment

The app is deployed via **GitHub Pages**:

1. Push `index.html` to the `main` branch of your GitHub repo
2. Go to **Settings → Pages → Source → main / root**
3. GitHub builds and deploys automatically — live at `https://yourusername.github.io/reponame`

To update the app, simply edit `index.html` on GitHub (or push a new version) and it's live within ~1 minute.

---

## Troubleshooting

### Google sign-in shows "Access blocked: redirect_uri_mismatch"
Your app's URL isn't in the authorized origins list. Go to Google Cloud Console → Credentials → your OAuth Client → add `https://yourusername.github.io` to Authorized JavaScript Origins (no trailing slash).

### Sync dot turns red / sync fails
- The Google access token expires after ~1 hour. The app will re-authenticate automatically on the next action.
- If it persists, tap **📊** → sign out → sign back in.

### KevAI shows "Couldn't reach proxy"
- Check that `KEVAI_PROXY_URL` is set correctly in `index.html` (no trailing slash, includes `https://`)
- Visit your Worker URL directly in a browser — you should see a response (even an error is fine)
- Make sure `ANTHROPIC_KEY` is saved as a **Secret** (not a plain variable) in Cloudflare Worker settings

### KevAI shows "No credits"
Add billing at [console.anthropic.com/settings/billing](https://console.anthropic.com/settings/billing). A $5 top-up covers ~800 analyses.

### Transactions not showing in sheet
The sheet sync requires an active Google session. Re-open the app and let the startup sync run (takes ~2 seconds after load). If the sync dot turns green, the sheet is up to date.

### "Local data is empty — upload blocked"
This safety guard prevents accidentally wiping a populated sheet with an empty local state. Add at least one source or transaction before uploading.

### App shows blank page on GitHub Pages
Make sure the file is named exactly `index.html` (not `finance-tracker.html`) and GitHub Pages is set to deploy from `main` branch, `/ (root)` folder.

---

## Currency Support

Vault supports multiple currencies (display only — no conversion):

| Symbol | Currency |
|---|---|
| ₱ | Philippine Peso (default) |
| $ | US Dollar |
| € | Euro |
| £ | British Pound |
| ¥ | Japanese Yen |
| ₩ | Korean Won |

Change currency in **Settings → Currency**.

---

*Built with Claude by Anthropic · Hosted on GitHub Pages · Data owned by you*
