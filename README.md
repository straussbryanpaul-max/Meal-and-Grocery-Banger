# Meal & Grocery Banger

A grocery list + recipe app. Single-file web app — open `index.html` and go. No build step required.

---

## What it does

| View | Description |
|------|-------------|
| **Staples** | The stuff you always buy — milk, eggs, coffee. Add once, tap any item to edit its name/qty/category. Always included in the trip list. |
| **Recipes** | Save your go-to meals with ingredients (amount + unit) and instructions. Paste a recipe from anywhere and let AI fill in the structured fields for you. |
| **Check** | Occasional stuff — olive oil, batteries, foil. Not bought every trip; reviewed each time and only added if you say you're out. |
| **Trip** | Hit "New Trip", pick which meals you're making and what you're out of (with a "last time" shortcut to reuse your previous picks), and get a shopping list = staples + those recipes' ingredients + anything marked out, merged and grouped by aisle. Check items off as you shop, and if you run out of something mid-week, add it straight to the active list without losing your progress. |

---

## Quick start

```bash
# Option 1 — just open the file
open index.html

# Option 2 — serve locally
npx serve . -p 3000
# then open http://localhost:3000
```

## Cloud sync setup

Tap the **gear icon** (top right) to enable sync across devices:

1. Create a free account at [jsonbin.io](https://jsonbin.io)
2. Create a new bin with initial content: `{"staples":[],"recipes":[],"checkItems":[]}`
3. Paste the **Bin ID** and **Master Key** into Settings

Credentials are stored in `localStorage` only. See `.env.example` for reference.

## AI recipe auto-fill

In the recipe editor, paste raw recipe text (from a website, note, text message — anything) into the "Paste a recipe to auto-fill" box and hit **Auto-fill with AI**. It extracts the name, ingredients (with amount, unit, and category), and instructions into the structured fields for you to review and save.

Requires an Anthropic API key — get one at [console.anthropic.com](https://console.anthropic.com) and paste it into Settings. Calls go directly from the browser to `https://api.anthropic.com/v1/messages` (model `claude-opus-5`, structured JSON output). The key is stored in `localStorage` only.

---

## Architecture

```
index.html          ← entire app (HTML + CSS + JS)
data-schema.json     ← all data structures documented
.env.example          ← credentials reference
```

### Storage

| What | Where |
|------|-------|
| Staples, recipes, check items | `localStorage`, synced to JSONBin.io |
| Active trip (selected meals, checks-out, checked items) | `localStorage` only, device-local |
| Last trip's meal picks (for the "last time" shortcut) | `localStorage` only, device-local |
| Anthropic API key | `localStorage` only, device-local |

`localStorage` keys are prefixed `mg_`. See `data-schema.json` for full schemas.

---

## Deploying to Vercel

This is a static site — no build step.

1. Import this repo at [vercel.com/new](https://vercel.com/new)
2. Framework preset: **Other**
3. Build command / output directory: leave blank
4. Deploy

---

## Extending

- No framework dependencies — all state is in-memory JS + localStorage.
- The trip list does NOT merge duplicate ingredient names — if "Milk" is both a staple and a recipe ingredient, it shows up as two separate rows sorted next to each other. Recipe-origin rows carry a badge naming the recipe (and amount/unit) so you can tell at a glance why an item's on the list.
