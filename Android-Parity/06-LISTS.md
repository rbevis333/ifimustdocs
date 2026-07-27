# 06 — List pages (Contacts / Recent / Favorites)

**Screenshots:** `07-all-contacts.png`, `08-recent-past-week.png`, `09-favorites.png`

Opened from Home’s three black buttons.

Shared chrome:

- Circular **back** button leading
- Inline title (or title control)
- White background, plain list, hairline separators
- Tap row → that node’s Master page

---

## All Contacts (`07-all-contacts.png`)

| Aspect | Spec |
|--------|------|
| Title | **All Contacts** |
| Contents | Contact masters only (not orphan attributes) |
| Sort | Alphabetical sections (`J`, `L`, `R`, `T`, …) |
| Section headers | Single letter, grey |
| Row | Display name (often lowercase) + trailing `>` |
| Index | A–Z jump index on trailing edge (letters that exist) |
| Empty | “No contacts yet” style empty state |

**Not** on these rows: red X, green +, reorder grip.

---

## Recent / Recently Added (`08-recent-past-week.png`)

| Aspect | Spec |
|--------|------|
| Home button label | **Recent** |
| Center title control | Menu showing **Past Day** / **Past Week** / **Past Month** |
| Default range | **Past Week** |
| Filter meaning | Contact masters with `dateCreated` in that window, newest first |
| Row | **Name** (primary) + **date** under it (caption/secondary) + trailing `>` |
| Empty | “No recent contacts” with range-specific message |

Screenshot title reads **Past Week** because the principal control shows the active range (not the static string “Recent”).

---

## Favorites (`09-favorites.png`)

| Aspect | Spec |
|--------|------|
| Title | **Favorites** |
| Row | Name + date subtitle + `>` + **reorder grip** on the far right |
| Reorder | Drag via grip; persist favorite order |
| **Not** included | Red disconnect X, green primary + |

Favoriting is toggled on the Master page star; this list only shows pinned favorites.

---

## Android parity traps

- [ ] Contacts list missing A–Z sections / index
- [ ] Recent has no Past Day/Week/Month control
- [ ] Favorites missing grip (or wrongly adding X/+)
- [ ] Showing attribute-only nodes in All Contacts
