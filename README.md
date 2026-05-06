# 💸 SpendSense

> A personal expense tracker built as a Progressive Web App — works entirely in Safari on iPhone, stores all data locally on your device, and can be added to your home screen like a native app.

**Live App → [LoginNoJutsu.github.io/SpendSense](https://LoginNoJutsu.github.io/SpendSense)**

---

## What it does

SpendSense helps you track your daily spending by either manually adding transactions or pasting a bank SMS and letting the app parse it automatically. All data is stored on your device — nothing is sent to any server.

---

## Features

- **Today view** — see your total spend for the day, top category, and a colour-coded category breakdown bar
- **SMS parser** — paste any bank or UPI SMS and SpendSense auto-detects the amount, merchant, and category
- **16 categories** — Food, Groceries, Travel, Fuel, Health, Shopping, Bills, Entertainment, Education, Rent, Salary, Personal, Subscriptions, Investment, Transfer, Other
- **Credit card tracking** — separate tab for Kotak, ICICI, Axis, LazyPay and other cards with one-time vs EMI split per card
- **History** — browse any past month, filter by category, grouped by day with daily totals
- **Category analytics** — monthly breakdown showing spend and transaction count per category
- **Edit & delete** — tap any transaction to edit amount, category, description, payment method or date
- **Hidden timestamps** — date and time are hidden by default, tap the 🕐 icon to reveal
- **Export** — download transactions as CSV (opens in Excel) or HTML report (printable as PDF)
- **Reset day** — reset the daily counter without deleting any history
- **Works offline** — no internet needed after first load
- **Add to home screen** — open in Safari → Share → Add to Home Screen for a full-screen app experience

---

## How to use on iPhone

1. Open **[LoginNoJutsu.github.io/SpendSense](https://LoginNoJutsu.github.io/SpendSense)** in Safari
2. Tap the **Share** button (box with arrow at the bottom)
3. Tap **Add to Home Screen**
4. Tap **Add** — SpendSense now lives on your home screen like a native app

---

## Adding a transaction

**Manually:**
- Tap the **+** button (bottom right)
- Fill in description, amount, category, payment method
- For credit card payments, select the card and whether it's a one-time or EMI purchase
- Tap Save

**From an SMS:**
- Open your bank's SMS on iPhone → copy it
- Tap the **📋** button (bottom right)
- Long press in the text box → Paste
- Tap Parse & Review — SpendSense fills in the details automatically
- Confirm and save

---

## Data & privacy

- All data is stored in your browser's `localStorage` — it never leaves your device
- No account, no login, no server, no tracking
- To back up your data, use the Export CSV button in the Categories tab
- Do not clear Safari website data for this site or your transactions will be erased

---

## Tech stack

| | |
|---|---|
| **Frontend** | Vanilla HTML, CSS, JavaScript — single file, zero dependencies |
| **Storage** | Browser localStorage |
| **Hosting** | GitHub Pages (free) |
| **Fonts** | DM Sans + DM Mono via Google Fonts |

---

## Project structure

```
SpendSense/
└── index.html    ← the entire app (HTML + CSS + JS in one file)
```

---

## Making updates without losing data

Since all user data lives in `localStorage` on the device (not in the code), you can update `index.html` freely without affecting anyone's saved transactions. The app uses the storage key `ss_v3` — as long as future updates don't change or clear this key, all existing data is preserved.

**Recommended workflow for any change:**

```bash
# 1. Create a feature branch
git checkout -b feature/your-feature-name

# 2. Make your changes to index.html

# 3. Commit
git add .
git commit -m "Describe what you added"

# 4. Push and open a Pull Request on GitHub
git push origin feature/your-feature-name
```

Merge the PR → GitHub Pages redeploys automatically within 1–2 minutes.

---

## Planned features

- [ ] Budget limits per category with overspend alerts
- [ ] Recurring expense tracking
- [ ] Monthly spending comparison chart
- [ ] Split expense tracking
- [ ] Data backup and restore via JSON export/import
- [ ] Multi-currency support
- [ ] Custom categories

---

## Contributing

This is a personal project but suggestions are welcome. Open an issue describing what you'd like to see added.

---

## License

MIT — free to use, modify and distribute.
