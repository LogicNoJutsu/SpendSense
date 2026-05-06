# SpendSense — Complete Code Explanation

Your entire app lives in one single file: `index.html`.
That file has 3 parts inside it — **HTML**, **CSS**, and **JavaScript**.
Think of them like this:

| Part | What it does | Real world analogy |
|---|---|---|
| HTML | The structure — what exists on the page | The skeleton of a building |
| CSS | The appearance — how it looks | The paint, furniture, lighting |
| JavaScript | The behaviour — what happens when you interact | The electricity and plumbing |

---

## SEGMENT 1 — The `<head>` (Lines 1–9)
### What it is: Setup instructions for the browser

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<title>SpendSense</title>
<link href="https://fonts.googleapis.com/..." rel="stylesheet">
```

**Line by line:**

`<!DOCTYPE html>` — Tells the browser "this is a modern HTML5 file". Always the first line of any HTML file.

`<html lang="en">` — The root tag that wraps everything. `lang="en"` tells screen readers the language is English.

`<meta charset="UTF-8">` — Allows the app to display special characters like ₹, emojis 🍽️, and symbols. Without this, those would show as garbage characters.

`<meta name="viewport"...>` — This is the most important line for mobile. It tells the browser: "don't zoom out to fit a desktop layout, show it at full iPhone width". Without this your app would look tiny and zoomed out on a phone.

`maximum-scale=1.0, user-scalable=no` — Prevents the user from accidentally zooming in by double tapping.

`apple-mobile-web-app-capable` — This is the magic line that makes your app feel native on iPhone. When someone adds it to their home screen, it opens without Safari's address bar — full screen like a real app.

`apple-mobile-web-app-status-bar-style: black-translucent` — Makes the iPhone status bar (time, battery, signal) transparent so your dark app background shows through it.

`<link href="fonts.googleapis.com...">` — Loads two custom fonts from Google: **DM Sans** (the main text font) and **DM Mono** (used for numbers and amounts). This is why your money figures look clean and aligned.

---

## SEGMENT 2 — CSS Variables (Lines 11–32)
### What it is: Your app's design system — all colours and sizes in one place

```css
:root {
  --bg: #0f0f13;
  --surface: #1a1a22;
  --accent: #7c6ff7;
  --red: #f87171;
  --radius: 14px;
  ...
}
```

`:root` means "apply these to the entire page". The `--` prefix means these are **CSS variables** — reusable values you define once and use everywhere.

**Why this matters for you:** If you want to change the purple accent colour of the whole app, you only change `--accent: #7c6ff7` in one place and every button, active tab, and highlight updates automatically. Without variables you'd have to find and replace that colour in 50+ places.

**The colour groups:**
- `--bg`, `--surface`, `--surface2`, `--surface3` — background layers (darkest to slightly lighter, creating depth)
- `--text`, `--text2`, `--text3` — text colours (bright → medium → dim, for hierarchy)
- `--accent` — the purple used for active states and buttons
- `--green`, `--red`, `--amber`, `--blue` — used for income, expenses, EMI badges, card labels
- `--radius`, `--radius-sm`, `--radius-lg` — the rounded corner sizes used on cards and buttons

---

## SEGMENT 3 — CSS Base Styles (Lines 33–34)
### What it is: Global resets and body setup

```css
* { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
body { font-family: 'DM Sans'; background: var(--bg); max-width: 430px; margin: 0 auto; }
```

`* { margin: 0; padding: 0 }` — Browsers add default spacing to elements. This removes all of it so you start with a clean slate.

`box-sizing: border-box` — Changes how width is calculated. By default if you say an element is 300px wide and add 20px padding, it becomes 340px. With border-box, it stays 300px and the padding fits inside. Much more predictable.

`-webkit-tap-highlight-color: transparent` — On iPhone, tapping a button normally shows a grey flash. This removes that flash so your app feels smooth.

`max-width: 430px; margin: 0 auto` — Caps the app at 430px (iPhone width) and centers it. If someone opens it on a desktop, it won't stretch weirdly across the screen.

---

## SEGMENT 4 — Navigation Bar CSS (Lines 36–47)
### What it is: Styles for the top bar and tab buttons

```css
.nav-bar { position: fixed; top: 0; ... backdrop-filter: blur(20px); }
.tab { flex: 1; ... border-bottom: 2px solid transparent; }
.tab.active { color: var(--accent); border-bottom-color: var(--accent); }
.page { display: none; }
.page.active { display: block; }
```

`position: fixed; top: 0` — The nav bar stays at the top even when you scroll down. Without `fixed` it would scroll away.

`backdrop-filter: blur(20px)` — Creates the frosted glass effect behind the nav bar. When you scroll content behind it, it blurs rather than showing sharp text underneath. This is a modern iOS-style effect.

`z-index: 100` — Layers. Higher z-index = appears on top. The nav bar (100) sits above the content. Modals use z-index 300 so they appear above everything.

`.tab.active` — The dot after `.tab` means "only apply this when the element has BOTH classes: tab AND active". The purple underline and colour only show on the currently selected tab.

`.page { display: none }` — All tab pages are hidden by default. Only the one with `.page.active` is visible. JavaScript adds/removes the `active` class to switch tabs.

---

## SEGMENT 5 — Summary Card, Transaction Items, FAB CSS (Lines 49–86)
### What it is: The visual design of your main UI components

**Summary card:**
```css
.summary-card { background: linear-gradient(135deg, #1e1b4b 0%, #1a1a2e 100%); }
```
`linear-gradient` creates the purple-to-dark gradient on the Today card. `135deg` is the angle (diagonal). `0%` and `100%` are where each colour starts and ends.

**Transaction items:**
```css
.txn-item { display: flex; align-items: center; gap: 12px; }
.txn-info { flex: 1; min-width: 0; }
```
`display: flex` arranges the icon, text, and amount side by side in a row. `align-items: center` vertically centres them. `flex: 1` on `.txn-info` means "take up all remaining space". `min-width: 0` allows the text to truncate with `...` instead of overflowing.

**FAB (Floating Action Button):**
```css
.fab-area { position: fixed; bottom: 30px; right: 20px; }
.fab:active { transform: scale(0.93); }
```
`position: fixed; bottom; right` pins the + button to the bottom right corner permanently. `transform: scale(0.93)` shrinks the button slightly when pressed, giving it a satisfying physical press feeling.

---

## SEGMENT 6 — Modal CSS (Lines 87–111)
### What it is: The slide-up sheet that appears when you add/edit a transaction

```css
.modal-bg { position: fixed; inset: 0; background: rgba(0,0,0,0.7); display: none; }
.modal-bg.show { display: flex; }
.modal { border-radius: 24px 24px 0 0; animation: slideUp 0.3s ease; }
@keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }
```

`position: fixed; inset: 0` — The dark overlay covers the entire screen. `inset: 0` is shorthand for `top:0; right:0; bottom:0; left:0`.

`background: rgba(0,0,0,0.7)` — Semi-transparent black. `rgba` = red, green, blue, alpha (opacity). 0.7 means 70% opaque — you can still faintly see the content behind.

`display: none` → `display: flex` — The modal is invisible by default. JavaScript adds the `show` class to make it appear.

`border-radius: 24px 24px 0 0` — Rounds only the top two corners, giving the iOS bottom sheet look. The bottom corners stay sharp because the sheet extends to the screen edge.

`@keyframes slideUp` — This defines the animation. `from` is the starting state (off-screen below), `to` is the end state (in position). `translateY(100%)` moves it down by 100% of its own height.

---

## SEGMENT 7 — HTML Structure: Nav Bar and Pages (Lines 179–269)
### What it is: The visible skeleton of your app

```html
<div class="nav-bar">
  <div class="nav-title">Spend<span>Sense</span></div>
  <div class="tabs">
    <div class="tab active" onclick="switchTab('home')">Today</div>
    ...
  </div>
</div>

<div class="content">
  <div class="page active" id="page-home"> ... </div>
  <div class="page" id="page-history"> ... </div>
  <div class="page" id="page-cats"> ... </div>
  <div class="page" id="page-cards"> ... </div>
</div>
```

`onclick="switchTab('home')"` — When you tap a tab, it calls the JavaScript function `switchTab` and passes it the name `'home'`. That function then shows the right page and hides the others.

`id="page-home"` — Every page has a unique ID so JavaScript can find it with `document.getElementById('page-home')`.

`id="today-total"`, `id="today-count"` etc. — These are empty placeholders. JavaScript fills them in with real data when the page loads.

---

## SEGMENT 8 — FAB and Modals HTML (Lines 271–415)
### What it is: The buttons and forms

```html
<div class="fab-area">
  <div class="fab fab-add" onclick="openAddModal()">+</div>
  <div class="fab fab-paste" onclick="openPasteModal()">📋</div>
</div>

<div class="modal-bg" id="add-modal">
  <div class="modal">
    <input class="form-input" id="f-desc" placeholder="e.g. Swiggy order..." />
    <select class="form-select" id="f-type">
      <option value="debit">Debit</option>
      <option value="credit">Credit</option>
    </select>
  </div>
</div>
```

`<input type="number" inputmode="decimal">` — `type="number"` restricts to numbers only. `inputmode="decimal"` tells iPhone to show the number keyboard with a decimal point instead of the full keyboard.

`<select>` and `<option>` — Creates a dropdown menu. The `value` attribute is what gets stored in your data (`"debit"`), while the text between the tags (`Debit`) is what the user sees.

`id="cc-extra-fields" style="display:none"` — The credit card fields are hidden by default. When you pick "Credit Card" from the payment method dropdown, JavaScript removes the `display:none` to reveal them.

---

## SEGMENT 9 — JavaScript: Data (Lines 395–441)
### What it is: Your categories, cards, and database

```javascript
const CATS = [
  { id:'food', name:'Food', icon:'🍽️', color:'#f97316' },
  { id:'travel', name:'Travel', icon:'🚌', color:'#3b82f6' },
  ...
];

const CAT_MAP = {};
CATS.forEach(c => CAT_MAP[c.id] = c);

let DB = JSON.parse(localStorage.getItem('spendsense_v2') || '{"txns":[]}');
```

`const CATS = [...]` — An **array** (list) of objects. Each object has 4 properties: id, name, icon, colour. This is your single source of truth for all categories. To add a new category, you just add a new object to this list.

`CAT_MAP` — A **lookup object**. Instead of searching through the whole CATS array every time you need to find "food", you can just write `CAT_MAP['food']` and get it instantly. The `forEach` loop builds this automatically.

`localStorage.getItem('spendsense_v2')` — This reads your saved data from the browser's local storage. It returns a JSON string (text). `JSON.parse()` converts that string into a real JavaScript object you can work with. The `|| '{"txns":[]}'` part means "if nothing is saved yet, start with an empty transactions array".

---

## SEGMENT 10 — JavaScript: Helper Functions (Lines 443–467)
### What it is: Small reusable utilities used throughout the app

```javascript
function saveDB() { localStorage.setItem('spendsense_v2', JSON.stringify(DB)); }
function uid() { return Date.now().toString(36) + Math.random().toString(36).slice(2,6); }
function notify(msg) { ... setTimeout(() => n.classList.remove('show'), 2000); }
function formatMoney(n) { return '₹' + Number(n).toLocaleString('en-IN', {...}); }
```

`saveDB()` — `JSON.stringify(DB)` converts your JavaScript object back into a text string so it can be stored. `localStorage.setItem` saves it. Called every time a transaction is added, edited, or deleted.

`uid()` — Generates a unique ID for each transaction like `lf2k3a9x`. `Date.now()` gives milliseconds since 1970, converted to base-36 (letters+numbers). Combined with random characters, collisions are virtually impossible.

`notify(msg)` — Adds the `show` class to the notification bar (making it slide down), then after 2000ms (2 seconds) removes it (making it slide back up).

`formatMoney(n)` — `toLocaleString('en-IN')` formats numbers in the Indian style: 1,00,000 instead of 100,000. `minimumFractionDigits:2` ensures you always see two decimal places (₹450.00 not ₹450).

---

## SEGMENT 11 — JavaScript: switchTab (Lines 469–481)
### What it is: Controls which tab is visible

```javascript
function switchTab(tab) {
  document.querySelectorAll('.tab').forEach((t,i) => {
    const tabs = ['home','history','cats','cards'];
    t.classList.toggle('active', tabs[i] === tab);
  });
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.getElementById('page-' + tab).classList.add('active');
  if(tab === 'home') renderHome();
  ...
}
```

`querySelectorAll('.tab')` — Finds ALL elements with the class `tab` and returns them as a list.

`classList.toggle('active', condition)` — If the condition is true, add `active`. If false, remove it. This is how the purple underline moves between tabs.

`classList.remove('active')` on all pages, then `classList.add('active')` on just the target — hides all pages, then shows only the one you want.

`renderHome()` etc. — Every time you switch to a tab, it re-renders the content with fresh data. This is why your totals always stay up to date.

---

## SEGMENT 12 — JavaScript: renderHome (Lines 494–537)
### What it is: Builds the Today tab from your data

```javascript
function renderHome() {
  const txns = getTodayTxns().sort((a,b) => new Date(b.datetime)-new Date(a.datetime));
  const debits = txns.filter(t => t.txn_type !== 'credit');
  const total = debits.reduce((s,t) => s + Number(t.amount), 0);
  document.getElementById('today-total').textContent = total.toLocaleString(...);
  list.innerHTML = txns.map(t => txnHTML(t)).join('');
}
```

`.sort((a,b) => new Date(b.datetime)-new Date(a.datetime))` — Sorts newest first. Subtracting two dates gives a number — positive means b is newer, negative means a is newer. The sort function uses this to order the list.

`.filter(t => t.txn_type !== 'credit')` — Keeps only debit transactions for the total. Credits (like salary received) are excluded from your spending total.

`.reduce((s,t) => s + Number(t.amount), 0)` — Adds up all amounts. `s` is the running sum, `t` is each transaction, `0` is the starting value.

`list.innerHTML = txns.map(t => txnHTML(t)).join('')` — `map` converts each transaction object into an HTML string using `txnHTML()`. `join('')` combines all those strings into one big HTML string. Setting `innerHTML` inserts it all into the page at once.

---

## SEGMENT 13 — JavaScript: txnHTML (Lines 539–558)
### What it is: Builds the HTML for a single transaction row

```javascript
function txnHTML(t) {
  const cat = CAT_MAP[t.category] || CAT_MAP.other;
  return `<div class="txn-item" onclick="openEditModal('${t.id}')">
    <div class="cat-icon" style="background:${cat.color}22">${cat.icon}</div>
    <div class="txn-desc">${t.desc}</div>
    <div class="txn-amount">${formatMoney(t.amount)}</div>
  </div>`;
}
```

This uses a **template literal** — the backtick string `` ` `` that allows you to embed variables directly using `${}`. Much cleaner than string concatenation.

`${cat.color}22` — Appends `22` to the hex colour code, which in hex means 13% opacity. So `#f97316` becomes `#f9731622` — the orange food colour at low opacity for the icon background. Clever trick.

`CAT_MAP[t.category] || CAT_MAP.other` — If the category doesn't exist in the map (maybe old data), fall back to the "other" category so it never crashes.

`onclick="openEditModal('${t.id}')"` — Each row has the transaction's unique ID baked into its click handler. When tapped, it passes that ID to `openEditModal` which knows exactly which transaction to load.

---

## SEGMENT 14 — JavaScript: saveTransaction (Lines 720–758)
### What it is: Saves a new or edited transaction

```javascript
function saveTransaction() {
  const txn = {
    id: currentEditId || uid(),
    desc, amount,
    datetime, date: datetime.slice(0,10)
  };
  if(currentEditId) {
    const idx = DB.txns.findIndex(t => t.id === currentEditId);
    if(idx >= 0) DB.txns[idx] = txn;
  } else {
    DB.txns.push(txn);
  }
  saveDB();
}
```

`currentEditId || uid()` — If editing an existing transaction, keep its original ID. If adding new, generate a fresh ID.

`DB.txns.findIndex(t => t.id === currentEditId)` — Searches the array for the transaction with a matching ID and returns its position number.

`DB.txns[idx] = txn` — Replaces the old transaction at that position with the updated one.

`DB.txns.push(txn)` — Adds the new transaction to the end of the array.

`saveDB()` — Immediately saves everything to localStorage so nothing is lost if the browser closes.

---

## SEGMENT 15 — JavaScript: parseSMS (Lines 760–820)
### What it is: The SMS auto-detection engine

```javascript
function parseSMS() {
  const amtPatterns = [
    /(?:Rs\.?|INR|₹)\s*([0-9,]+(?:\.[0-9]{1,2})?)/i,
    ...
  ];
  let cat = 'other';
  if(/zomato|swiggy|restaurant|food/i.test(text)) cat = 'food';
  else if(/ola|uber|metro|train/i.test(text)) cat = 'travel';
  ...
}
```

The `/ /` syntax is a **Regular Expression (Regex)** — a pattern for matching text.

`/(?:Rs\.?|INR|₹)\s*([0-9,]+)/i` breaks down as:
- `Rs\.?` — matches "Rs" or "Rs." (the `?` makes the dot optional, `\.` escapes it)
- `|INR|₹` — OR "INR" OR the ₹ symbol
- `\s*` — followed by zero or more spaces
- `([0-9,]+)` — capture one or more digits or commas (the actual amount)
- `/i` — case insensitive

`/zomato|swiggy|restaurant/i.test(text)` — Tests if any of those words appear anywhere in the SMS. Returns true or false. The `|` means OR.

This is how the app knows "Zomato" = Food and "Metro" = Travel automatically.

---

## SEGMENT 16 — JavaScript: Swipe Navigation (Lines 992–1023)
### What it is: The swipe left/right gesture for tab switching

```javascript
(function() {
  let txS = 0, tyS = 0, valid = false;
  area.addEventListener('touchstart', function(e) {
    txS = e.touches[0].clientX;
    tyS = e.touches[0].clientY;
    valid = true;
  });
  area.addEventListener('touchend', function(e) {
    const dx = e.changedTouches[0].clientX - txS;
    if(Math.abs(dx) < 50) return;
    if(dx < 0) switchTab(TABS[i + 1]); // swipe left
    if(dx > 0) switchTab(TABS[i - 1]); // swipe right
  });
})();
```

`(function(){ ... })()` — An **IIFE** (Immediately Invoked Function Expression). Creates a private scope so the variables `txS`, `tyS` don't accidentally conflict with other variables in the app. The `()` at the end runs it immediately.

`touchstart` — Fires the moment your finger touches the screen. Records the starting X position.

`touchend` — Fires when your finger lifts. Calculates how far you moved.

`dx` = end X minus start X. If negative, you moved left. If positive, you moved right.

`Math.abs(dx) < 50` — Ignores tiny movements under 50px. Prevents accidental tab switches when you meant to tap.

`valid = false` in `touchmove` — If you're clearly scrolling vertically (dy > dx), it cancels the swipe tracking so vertical scrolling still works normally.

---

## SEGMENT 17 — JavaScript: iOS URL Scheme (Lines 975–990)
### What it is: Reads SMS from iPhone Shortcuts via the URL

```javascript
function checkURLParams() {
  const params = new URLSearchParams(window.location.search);
  const smsText = params.get('sms');
  if(smsText) {
    document.getElementById('sms-text').value = decodeURIComponent(smsText);
    document.getElementById('paste-modal').classList.add('show');
    window.history.replaceState({}, '', window.location.pathname);
  }
}
```

`window.location.search` — The part of the URL after the `?`. For `spendsense.com?sms=Hello` this would be `?sms=Hello`.

`URLSearchParams` — A built-in browser tool that parses URL parameters cleanly.

`params.get('sms')` — Extracts the value of the `sms` parameter.

`decodeURIComponent` — URLs can't contain spaces or special characters, so they get encoded as `%20`, `%E2%82%B9` etc. This decodes them back to normal text.

`window.history.replaceState` — Cleans the URL after reading it, so if you refresh the page it doesn't try to parse the SMS again.

---

## How all 3 parts work together — a real example

When you tap **+ to add a transaction**, here's what happens across all 3 parts:

1. **HTML** — The `+` button has `onclick="openAddModal()"` which calls JavaScript
2. **JavaScript** — `openAddModal()` adds the class `show` to `#add-modal`
3. **CSS** — `.modal-bg.show { display: flex }` makes the modal visible with the slide-up animation
4. You fill in the form and tap Save
5. **JavaScript** — `saveTransaction()` reads all the input values, creates a transaction object, pushes it to `DB.txns`, calls `saveDB()` to write to localStorage
6. **JavaScript** — `renderHome()` is called, which builds fresh HTML strings from the updated data
7. **HTML** — `list.innerHTML = ...` inserts that HTML into the page
8. **CSS** — The new transaction row is styled by `.txn-item`, `.cat-icon` etc. and appears instantly

---

## Things you can easily change yourself

Now that you understand the code, here are changes you can make confidently:

**Change the app's accent colour:**
Find `--accent: #7c6ff7` in the `:root` block and change the hex code.

**Add a new category:**
Find the `CATS` array and add a new object:
```javascript
{ id:'gym', name:'Gym', icon:'🏋️', color:'#06b6d4' },
```

**Add a new credit card:**
Find the `CC_CARDS` array and add:
```javascript
{ id:'hdfc', name:'HDFC', color1:'#1a3c6e', color2:'#0f2447' },
```
Then add `<option value="hdfc">HDFC</option>` to the card dropdown in the HTML.

**Change the SMS keyword detection:**
Find the `if(/zomato|swiggy/i.test(text))` section and add new keywords to any category.

**Change the swipe distance:**
Find `Math.abs(dx) < 50` and increase the number to require a longer swipe.
