# 07 — Android parity checklist

Use after implementing or fixing each area. Check against the named screenshot.

## Global

- [ ] Trailing chrome is **gear**, not profile (`01-home.png`)
- [ ] Menu + gear sit in **circular** white buttons with light shadow
- [ ] Title **If I Must** returns Home from nested screens
- [ ] Master shows circular **back** when pushed (`10-master-top.png`)
- [ ] Light theme: white pages, black primary buttons, grey secondary buttons
- [ ] No modifier syntax UI; no chip-cloud connections; no Quick Add as primary UX

## Home (`01`–`03`)

- [ ] Order: Add → hints? → Search → helper? → results box → 3 buttons → Drafts → encouragement
- [ ] Empty search box shows centered empty copy
- [ ] Buttons labeled **Contacts / Recent / Favorites** (or icons when Symbols On)
- [ ] Minimalist hides hints, search helper, Drafts title
- [ ] Symbols swaps the three browse buttons to person / hourglass / star

## Menu (`04`)

- [ ] Rows in documented order; Home blue without chevron; Done dismisses
- [ ] No Benchmarks in production Play build

## Settings (`05`–`06`)

- [ ] Appearance: Hints, Symbols, Minimalist, Passcode, Theme, Grip Hand
- [ ] Font size + / − with footnote
- [ ] Delete Empty Containers = red bar, black text
- [ ] Backup email + export + import + footnote

## Master (`10`–`12`) — highest priority

- [ ] **Connections/Attributes below** Word/Link/Make (+ Favorites)
- [ ] Word default selected (black); Link/Make grey
- [ ] Sentence radio right of Add Info in Word only; default off
- [ ] Connection row: red **filled-circle** X · blue name · green **filled-circle** + (if not primary) · grip
- [ ] Grip Hand Left mirrors correctly
- [ ] Primaries omit green +
- [ ] Call/Text/Email grey→black by validity
- [ ] Bottom Phone / Demote|Promote / **red Delete**
- [ ] Dates + encouragement under buttons
- [ ] Minimalist + Symbols match `12-master-symbols-minimalist.png`

## Lists (`07`–`09`)

- [ ] All Contacts: A–Z sections + index; title All Contacts
- [ ] Recent: Past Day/Week/Month control; name + date rows
- [ ] Favorites: name + date + chevron + grip; no X/+

## Reorder (`08-REORDER-DRAG.md`)

- [ ] Dragging moves **full row** (X + name + green + + grip), not grip alone
- [ ] Other rows stay put until release; order persists

## Menu copy

- [ ] How-To matches `HOW-TO-GUIDE.md` including **Watch Tutorial Video** → YouTube channel
- [ ] Why I Built matches `WHY-I-BUILT-THIS-APP.md` word-for-word (10 paragraphs)

---

## Done criteria for a screen

A screen is “done” only when:

1. Vertical/horizontal order matches the markdown, and  
2. A side-by-side with the screenshot shows the same controls (no missing green +, grip, headers, or wrong placement), and  
3. Behaviors in the markdown work (tap, toggle, navigate, empty states).
