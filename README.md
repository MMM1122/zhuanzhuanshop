# Lucky Spinners · 幸运转转

A bilingual storefront for hand-glazed ceramic spinning figurines from Jingdezhen.
Campus pickup at UBC, reserved with an e-Transfer deposit.

**Live:** https://mmm1122.github.io/zhuanzhuanshop/

---

## What this is

A single-file static shop. All the code, styling, and 24 product photos are packed
into one `index.html` — no backend, no database, no build step, no dependencies.

Double-click to open it locally, or drop it on any static host to go live.

## Features

| | |
|---|---|
| **Bilingual** | English / 中文 toggle, defaults to browser language |
| **Category filter** | Seven collections, one click each |
| **Product detail** | Click a photo for a 360° stage where the piece keeps turning |
| **Spin effects** | Hover makes a product rotate like it's on a turntable, with a glaze shine sweeping across |
| **Cart drawer** | Live deposit and balance calculation |
| **e-Transfer checkout** | Auto-generated order ID and transfer message, click to copy |
| **Stock control** | One of each — adding to cart locks it; sold items get a veil |

## Catalogue & pricing

| Collection | 中文 | Price | Count |
|---|---|---|---|
| Siamese cats | 暹罗猫 | $21.99 | 4 |
| Bubu & Dudu | 布布嘟嘟 | $26.99 | 8 |
| Shin-chan | 小新 | $26.99 | 3 |
| Twinkle Twinkle | 星星人 | $26.99 | 1 |
| Others | 其他 | $26.99 | 3 |
| Chiikawa | 吉伊卡哇 | $28.99 | 3 |
| Large dolls | 大娃娃 | $34.99 | 2 |

24 designs total, one of each.

## How an order works

1. Customer browses and adds items to the cart
2. At checkout they enter a contact; the site generates a transfer message
3. They send a **$5 deposit** by Interac e-Transfer, pasting that message in
4. Seller contacts them the same day and meets at **UBC AMS Nest** the next day;
   the balance is paid in person

Message format:

```
ZZ-483920 | Cotton Doll · Mint + Siamese Cat · Reading | 778-123-4567
   order ID              what they bought                  contact
```

---

## How to edit

All settings sit at the top of the `<script>` block — search for `卖家配置`
(seller config) to find them.

### Email, deposit, pickup spot

```javascript
const SELLER_EMAIL = 'mmeghan2900@gmail.com';
const DEPOSIT      = 5;
const PICKUP_SPOT  = 'UBC AMS Nest';
```

### Prices

Set per collection, so you don't touch individual products:

```javascript
const PRICE = {
  big:   34.99,   // Large dolls
  bubu:  26.99,   // Bubu & Dudu
  chii:  28.99,   // Chiikawa
  shin:  26.99,   // Shin-chan
  cat:   21.99,   // Siamese cats
  twink: 26.99,   // Twinkle Twinkle
  other: 26.99,   // Others
};
```

### Marking something sold

Find the product in the `PRODUCTS` array and add `sold:true`:

```javascript
{id:'cat-book', img:'cat-book', cat:'cat', price:PRICE.cat, sold:true,
 zh:['暹罗猫 · 看书','《训服人类》'], en:['Siamese Cat · Reading','"How to Train Humans"']},
```

The card then shows a "Sold out" veil, stops animating, and disables the add button.

### Copy and text

English and Chinese live in `DICT.en` and `DICT.zh`. **Every key must exist in both** —
a missing one shows up as `undefined` when the language is switched.

Product names live in `PRODUCTS`, two entries each:

```javascript
zh:['中文名','副标题'], en:['Product name','Subtitle']
```

---

## Deploying

Any of these, all free:

**Netlify Drop** — fastest, no git needed.
Rename the file to `index.html` and drag it to https://app.netlify.com/drop

**GitHub Pages** — what this site currently uses.
Push to the repo → Settings → Pages → Source: main branch → Save

**Cloudflare Pages / Vercel** — connect the GitHub repo for automatic deploys

---

## Design notes

### Why a single file

The 24 product photos are embedded as base64 and account for roughly 98% of the
file (~1.9 MB). The cost is a larger file and a slightly slower first load. What it
buys is **zero external dependencies** — no broken image paths, no CDN outages,
nothing to misconfigure.

At this catalogue size that trade is worth it. Past a few hundred products, the
images should move to external files.

### Why e-Transfer instead of card payments

A static site has no backend, so accepting cards would mean putting API keys in
front-end code where anyone can read them via View Source. e-Transfer keeps the
money entirely inside the banking system; the site only displays information and
composes an order message, and never touches payment credentials.

### The stock limitation ⚠️

**This is the real constraint of a static site.** With no backend, stock exists only
in each visitor's own browser — two people can open the page at the same time, both
add the same item, and both get transfer instructions.

The current approach is **first transfer wins**, with a full refund to whoever came
second. That's stated on the checkout screen.

Solving it properly needs a backend (Supabase, or a Google Sheet + Apps Script)
decrementing stock in real time.

### Photo processing

The originals are phone photos, put through a consistent pass: grey-world white
balance, exposure lift, a radial background blend (fading the grey wall into the
site's ice-blue theme), +20% saturation, and an unsharp mask.

Cropping **pads to square rather than cropping to square** — the latter cut the
tops off taller pieces (spoons, flowerpots, hats).

---

## Known limitations

1. **Stock doesn't sync across devices** — see above
2. **Sold items are marked manually** — edit the `sold` field
3. **No order dashboard** — reconciliation happens through e-Transfer messages in email
4. **No search** — category filters are enough for 24 items

## Structure

```
zhuanzhuanshop/
└── index.html        everything (code, styles, 24 product photos)
```
