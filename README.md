# (Ke)Vault — Personal Finance Tracker

> A mobile-first personal finance web app with Google Sheets sync, AI-powered insights, multi-account tracking, savings goals, and transfer management.

**Live at:** [kevinsornet.github.io/Kevault](https://kevinsornet.github.io/Kevault)

---

## ⚠️ Invite Only

**Vault is currently invite-only.** To get access, you'll need to be added to the app's authorized user list before you can sign in with Google.

To request access, contact **Kevin Sornet** directly and provide your Gmail address. Once added, you'll be able to sign in and use all features immediately.

> Your data is entirely your own — it lives in your browser and your personal Google Drive. Kevin has no visibility into your transactions, balances, goals, or any other information you enter into the app.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Getting Started](#getting-started)
- [Google Sheets Integration](#google-sheets-integration)
- [Kevinsights (KevAI)](#kevinsights-kevai)
- [Data & Privacy](#data--privacy)
- [Currency Support](#currency-support)
- [Troubleshooting](#troubleshooting)

---

## Overview

Vault is a personal budgeting app designed for everyday use on mobile and desktop. It runs entirely in the browser — no backend, no database, no login system of its own. Your data lives in your browser's `localStorage` and optionally syncs to a Google Sheet you own.

Built for people who want to:
- Log expenses, income, and transfers across multiple accounts quickly
- See where their money is going with clear visual breakdowns
- Set and track savings goals tied to specific accounts
- Get AI-generated financial insights through **Kevinsights**
- Keep a live backup in their own Google Sheets automatically

---

## Features

### 📊 Dashboard
- **Net worth** calculated live across all linked accounts
- **Monthly income vs. expense** summary cards
- **Transfer fee tracker** — total fees paid this month, number of transfers, and volume moved
- **6-month dual-bar chart** — income (green) and expenses (red) side by side
- **Sources summary** — all accounts with live balances, tap any to see full detail
- **Recent transactions** list

### 💸 Transactions
- Log **expenses**, **income**, and **transfers** between accounts
- **21 expense categories** — Food & Dining, Transport, Shopping, Housing, Health, Entertainment, Subscriptions, Phone Plan & Add-ons, Credit Card Payment, Investment, Loan Payment, Insurance, Travel, and more
- **11 income categories** — Salary, Business, Freelance, Investment Returns, Stock/Crypto, Dividend, Bonus, Refund/Cashback, and more
- **Edit** and **delete** any transaction
- **Transfer between accounts** with optional service fee — fee is tracked separately and shown in the transfer fee tracker

### 📈 Analytics
- **Month navigator** — browse any past month with ‹ › arrows
- **4 summary cards** — Income, Expenses, Net Saved, Savings Rate
- **Expense & income breakdowns** — color-coded category bars with amounts and percentages
- **Daily spending sparkline** — 31-bar chart showing which days you spent the most
- **Activity heatmap calendar** — full month grid color-coded by spending intensity; tap any day for a full breakdown of what happened that day
- **Kevinsights (KevAI)** — AI-powered monthly spending analysis

### 🎯 Goals
- Create savings goals with a name, emoji, target amount, deadline, and optional monthly contribution target
- **Link a goal to an account** — adding funds to a goal automatically deducts from the linked account and logs the transaction
- Visual progress bar with % complete, saved, and remaining
- Linked account and monthly target shown as badges

### 💳 Sources (Accounts)
- Add **Bank**, **E-Wallet**, **Cash**, **Credit Card**, and **Investment** accounts
- **Edit** name, type, starting balance, and color of any account at any time
- Balances calculated live: starting balance + income − expenses − transfers out + transfers in − fees
- **Tap any account** for a detailed view:
  - Balance summary (Total In, Total Out, Fees Paid)
  - 6-month activity sparkline
  - **Linked goals** — allocation bar showing how much of the current balance is allocated to each goal vs. unallocated, with full contribution history per goal
  - Transaction history grouped by month, filterable by All / Out / In

### 📗 Google Sheets Sync
- **Create a new sheet** from inside the app — automatically sets up Transactions, Goals, and Sources tabs with styled, frozen headers
- **Browse your Drive** — search and pick any existing sheet without leaving the app
- **Auto-sync on every change** — every add, edit, and delete instantly updates your sheet
- **Startup sync** — re-authenticates and syncs every time you open the app
- **5-minute background sync** during active sessions
- **Page-close sync** — final push when you close the tab
- **Safety guards** — upload is blocked if local data is empty; download warns you before overwriting local data
- **Sync status dot** in the header — grey (no sheet), purple (linked), amber (syncing), green (synced), red (error)

### ✦ Kevinsights (KevAI)
- AI-powered monthly financial analysis
- Returns three sections based on your actual spending data:
  - **What you're doing well** 🌟
  - **Watch out** ⚠️
  - **Tips to save more** 💡
- References your real numbers — specific categories, amounts, savings rate, and goal progress

---

## Getting Started

1. **Request access** — contact Kevin with your Gmail address to be added as an authorized user
2. Once added, open [kevinsornet.github.io/Kevault](https://kevinsornet.github.io/Kevault)
3. Tap **💳** in the header to add your first account
4. Tap **+** in the bottom nav to log your first transaction
5. Tap **📊** to connect your own Google Sheet for automatic backup

---

## Google Sheets Integration

When you tap 📊 and sign in with Google, Vault connects to your **personal Google Drive**. It can create a new spreadsheet or link to an existing one.

### What gets synced

| Tab | Contents |
|---|---|
| **Transactions** | Date, Type, Description, Category, Amount, Source |
| **Goals** | Name, Target, Saved, Progress, Deadline, Linked Account |
| **Sources** | Account Name, Type, Starting Balance |

### How sync works
- Every change you make (add, edit, delete) syncs to the sheet instantly
- On startup, the app checks both sides and picks the right action automatically
- A background sync runs every 5 minutes as a safety net
- You can always manually push or pull from the 📊 menu

---

## Kevinsights (KevAI)

Kevinsights is powered by **Claude** (Anthropic's AI). It analyzes your spending for the selected month and gives you personalized feedback — praising good habits, flagging concerns, and offering practical saving tips.

To use it, tap **Analyze ↻** in the Transactions tab. It usually takes 3–5 seconds to respond.

KevAI uses your monthly totals and category breakdowns to generate insights. It does **not** have access to individual transaction descriptions, account names, or any personally identifying information.

---

## Data & Privacy

Vault is designed with privacy as a default:

- **Your data never touches any shared server.** Everything is stored in your own browser (`localStorage`) and your own Google Drive.
- **Kevin cannot see your data.** There is no shared database, no analytics dashboard, no logging of transactions. The invite-only restriction is purely about Google OAuth authorization — not data access.
- **KevAI only sees aggregated numbers** — monthly totals and category sums. No transaction descriptions, no account names, no personal details.
- **Google Sheets access is scoped to files Vault creates or you explicitly select.** It cannot read the rest of your Drive.
- **No ads. No tracking. No third-party analytics** of any kind.

### To clear your data
Go to **Settings → Clear All Data**. This wipes everything from your browser. Your Google Sheet (if linked) is not affected.

---

## Currency Support

| Symbol | Currency |
|---|---|
| ₱ | Philippine Peso (default) |
| $ | US Dollar |
| € | Euro |
| £ | British Pound |
| ¥ | Japanese Yen |
| ₩ | Korean Won |

Change in **Settings → Currency**.

---

## Troubleshooting

### "Access blocked" when signing in with Google
You haven't been added as an authorized user yet. Contact Kevin with your Gmail address to request access.

### Sync dot turns red
Your Google session may have expired. Tap **📊** → sign out → sign back in. The app will re-sync automatically.

### Kevinsights shows an error
- Make sure you have an active internet connection
- Try tapping **Analyze ↻** again — occasional timeouts can happen
- If it persists, contact Kevin

### Transactions not appearing in my sheet
The sync runs ~1.5 seconds after the app loads while it re-authenticates with Google. Wait a moment after opening the app and check the sync dot — when it flashes green, everything is up to date.

### "Local data is empty — upload blocked"
This safety guard prevents accidentally overwriting your sheet with blank data. Add at least one account or transaction first.

---

*Built with ♥ by Kevin Sornet · Powered by Claude (Anthropic) · Hosted on GitHub Pages*
*Your data is yours — always.*
