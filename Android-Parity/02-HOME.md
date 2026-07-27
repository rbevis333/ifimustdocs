# 02 — Home page

**Screenshots:** `01-home.png`, `02-home-search-result.png`, `03-home-symbols-minimalist.png`

## Vertical order (top → bottom)

```
1. Nav: [☰]  If I Must  [⚙]
2. Add Contact          ← bordered text field, placeholder "Add Contact"
3. Show Hints           ← only if App Hints On AND Minimalist Off
   (optional expanded hint bullets)
4. Search               ← bordered text field, placeholder "Search"
5. Search Contacts/Attributes   ← helper; Minimalist Off only
6. Search results box   ← always present (empty state or results)
7. [ Contacts | Recent | Favorites ]   ← three equal black buttons
8. Drafts/Notes         ← section title; Minimalist Off only
9. Drafts preview row   ← grey bar + chevron (always when section shown)
10. Encouragement       ← "You're doing great. Have an amazing day."
```

Compare `01-home.png` (full labels) vs `03-home-symbols-minimalist.png` (no Show Hints, no Search helper, no Drafts/Notes title, icon buttons).

---

## 1. Add Contact

| Aspect | Behavior |
|--------|----------|
| Placeholder | `Add Contact` |
| Submit | Keyboard / toolbar **Add** runs add parser; creates/updates graph; clears field on success |
| Errors | Alert (e.g. Add Error) |
| Hints | Collapsible **Show Hints** / **Hide Hints** with bullet lines (format, concatenate with `.`, commas for multiple) when Hints On & not Minimalist |
| Characters | Current iPhone: allow broad input; `,` and `.` remain parser operators |

## 2. Search

| Aspect | Behavior |
|--------|----------|
| Placeholder | `Search` |
| Helper under field | `Search Contacts/Attributes` (hidden Minimalist) |
| Live search | Debounced as user types; results update in the grey box |
| Submit | Keyboard / floating **Search** records into Recent Searches |
| Result row | Display name of matching node (e.g. `ted williams` in `02-home-search-result.png`) |
| Tap result | Open that node’s **Master** page; clear/home search as iPhone does |
| Empty box copy | `Search results will appear here.` / `Start typing to refine results.` (centered in grey box) |
| Box chrome | Light grey fill, rounded corners; tall empty state; grows with results (capped) |

## 3. Contacts / Recent / Favorites

Black equal buttons. Labels when **Symbols Off**:

| Label | Opens |
|-------|--------|
| **Contacts** | All Contacts (`07-all-contacts.png`) |
| **Recent** | Recently Added / Past Week control (`08-recent-past-week.png`) |
| **Favorites** | Favorites (`09-favorites.png`) |

When **Symbols On** (`03-home-symbols-minimalist.png`):

| Slot | Icon meaning |
|------|----------------|
| Contacts | Person in circle |
| Recent | Hourglass |
| Favorites | Filled star |

**Not** labeled “All” on the button (list title is “All Contacts”).

## 4. Drafts/Notes

- Title `Drafts/Notes` (hidden Minimalist)
- Preview: `Tap to add drafts/notes...` + trailing `>`
- Tap → full-screen / sheet notes editor (home-scoped drafts)

## 5. Encouragement

Centered bold line under content:

> You're doing great. Have an amazing day.

Still visible in Minimalist.

---

## Minimalist + Symbols matrix (Home)

| Element | Default | Minimalist On | Symbols On |
|---------|---------|---------------|------------|
| Add / Search fields | Shown | Shown | Shown |
| Show Hints | If Hints On | Hidden | — |
| Search helper | Shown | Hidden | — |
| Results box | Shown | Shown | Shown |
| Three buttons | Text labels | Text or icons | Icons |
| Drafts title | Shown | Hidden | — |
| Drafts row | Shown | Shown | Shown |
| Encouragement | Shown | Shown | Shown |

---

## Android parity traps (Home)

- [ ] Missing Search results box when empty
- [ ] Button says “All” instead of **Contacts**
- [ ] Profile icon instead of gear
- [ ] Hints / helpers still visible with Minimalist On
- [ ] Drafts/Notes title still visible with Minimalist On
- [ ] No encouragement line
- [ ] Search does not open Master on row tap
