# If I Must — iPhone UI Parity Bible (source of truth for Android)

**Purpose:** Tell the Android agent exactly how the **current** iPhone app looks and behaves so Android can match it.  
**Source of truth:** Live SwiftUI under `/Users/randybevis/Desktop/If I Must/` (not early design sketches alone).  
**Repo:** This file lives in `ifimustdocs` → `Specs/IPHONE_UI_PARITY.md`.

**How to use on Windows:** Open this file first when fixing layout or missing features. Prefer this over early `DESIGN_REFERENCE.md` and over chat memory when they conflict.

---

## 0. Can the AI “see” the app? Do screenshots help?

| What the agent has | How good for parity |
|--------------------|---------------------|
| SwiftUI layout order, SF Symbols, paddings, conditionals | **Excellent** for structure, behavior, relative position |
| Design PNGs / early `DESIGN_REFERENCE.md` | **Historical** — some ideas were abandoned |
| Screenshots of **current** iPhone screens | **Excellent** for visual polish (spacing feel, sizes, empty vs filled states) |
| Watching the live app without screenshots | **No** — the agent cannot see your phone |

**Recommendation:** Yes — upload screenshots into the docs repo (e.g. `Design/screenshots/iphone/`) named clearly:

- `01-home-empty.png`
- `02-home-search-results.png`
- `03-home-symbols-on.png`
- `04-menu.png`
- `05-settings.png`
- `06-master-word.png`
- `07-master-link.png`
- `08-master-connections.png` (show red X circle, green +, grip)
- `09-master-contact-buttons.png`
- `10-master-minimalist.png`

Screenshots are optional but high value for “looks off” bugs. This document alone should fix **order**, **missing controls**, and **wrong icons**.

---

## 1. Evolution (early idea → current shipping UI)

### Early concept (design PNG / early DESIGN_REFERENCE)
- Header: hamburger · **If I Must** · **circular profile** icon  
- Master: big name “bubble”, **wrapping tag chips** for connections  
- Add Info + Additional Info / notes  
- **No** Word / Link / Make mode row as the primary edit chrome  
- Home hint mentioned concatenation with `.` and explicitly **no** `d*` / `p*` / `u*` modifiers  

### What was tried and removed (do **not** rebuild on Android)
- **Modifier prefixes** like `d*`, `p*`, `u*` — never shipping behavior  
- Standalone **Quick Add** screen (`AddView`) — orphaned; Home inline Add replaced it  
- **Copy** icons next to phone/email — tried then removed; long-press copy only if needed  
- Phone/email as **tappable `mailto:` / `tel:` links** — replaced by explicit **Call / Text / Email** buttons  
- Trailing chrome as **profile avatar** — replaced by **gear** (`gearshape.fill`) for App Settings  
- Connections as **wrapping chips** — replaced by **ordered list rows** with disconnect / primary / reorder  

### Current product model (match this)
- Graph of **Masters** (contact-capable nodes) and **attributes**, linked both ways  
- Home: Add + Search + browse lists + Drafts/Notes  
- Master: **Word / Link / Make** edit modes, connections list with **primary** (green +) and **reorder grip**, Contact block, General Info, bottom actions  
- Settings: Hints, Symbols, Minimalist, Passcode, Theme, Grip Hand, Font Size, Backup  
- Subscription gate on production; Test build bypasses paywall  

---

## 2. Global chrome (every main screen)

Same on **Home** and **Master**:

| Position | Control | Appearance | Action |
|----------|---------|------------|--------|
| Center | Title | Text **"If I Must"** · bold ~title3 | Tap → navigate to **Home** (clear stack) |
| Leading (left) | Menu | SF `line.3.horizontal` | Opens **Menu** sheet |
| Trailing (right) | Settings | SF `gearshape.fill` | Opens **App Settings** sheet |

**Not** a profile/avatar circle on the right.

---

## 3. Menu (hamburger) — top to bottom

Sheet titled **Menu**, Done to dismiss.

1. **Home** — go to Home, dismiss menu  
2. **How-To Guide**  
3. **App Settings** (same settings as gear)  
4. **Why I Built This App**  
5. **Recent Searches** (up to 20; empty state with magnifying glass)  
6. **App Info**  
7. **Contact**  
8. **Benchmarks** — **Test app only** (omit on Play production equivalent)

If Android is missing menu rows, add them in this order.

---

## 4. App Settings — top to bottom

### Appearance
1. **App Hints** — On / Off (default On)  
2. **Symbols** — On / Off (default Off) — when On, many black buttons show SF-style icons instead of text  
3. **Minimalist** — On / Off — hides helper labels / section headers / hints; fields and actions remain  
4. **Passcode** — On / Off  
5. **Theme** — Light / Dark  
6. **Grip Hand** — Left / Right — mirrors Master connection row controls  

### Text
7. Font size **+** / current % / **−** (+ iPad note footnote)

### Cleanup
8. Footnote about empty attribute containers  
9. **Delete Empty Containers** (destructive)

### Backup
10. Email field for backup destination  
11. **Email JSON backup**  
12. **Import JSON backup…**  
13. Footnotes about Mail / merge vs replace  

### Test only
14. **Delete All Contacts** (destructive)

---

## 5. Home page — top to bottom

```
[ Menu ]     If I Must     [ Gear ]
────────────────────────────────────
Add Contact          ← text field
[ Show Hints / hint bullets ]   ← if Hints On and not Minimalist
Search               ← text field
Search Contacts/Attributes      ← helper; hidden if Minimalist
┌ Search results box ┐          ← grey rounded; empty or list
└────────────────────┘
[ Contacts ] [ Recent ] [ Favorites ]   ← three equal black buttons
Drafts/Notes                            ← header; hidden if Minimalist
[ Tap to add drafts/notes...  › ]       ← grey preview row
You're doing great. Have an amazing day.
```

### Add Contact
- Placeholder: **Add Contact**  
- Submit (keyboard Add / Done): parse add rules → create/update graph; clear field  
- Character rules: Allow All for add/edit in current iPhone (parser still uses `,` and `.` as operators)  
- Collapsible hints (Format / Concatenate / commas) when App Hints On and not Minimalist  

### Search
- Placeholder: **Search**  
- Helper under field: **Search Contacts/Attributes** (not Minimalist)  
- Live debounce search; results in grey box  
- Tap result → Master page for that node  
- Empty box message: search results appear here / start typing  

### Three browse buttons
| Label (Symbols Off) | Symbol when Symbols On | Opens |
|---------------------|------------------------|--------|
| **Contacts** | `person.crop.circle` | All contacts list |
| **Recent** | `hourglass` | Recently added |
| **Favorites** | `star.fill` | Favorites |

How-To text may still say “All”; **UI label is Contacts**.

### Drafts/Notes
- Preview row opens full notes editor (home-scoped notes, large char limit)  

### Encouragement
- Fixed line at bottom of scroll content  

---

## 6. Master page — top to bottom (CRITICAL LAYOUT)

**Android bug to fix:** Connections/Attributes must be **below** Word / Link / Make (and Favorites), **not above**.

```
[ Menu ]     If I Must     [ Gear ]
────────────────────────────────────
[✏]  Display Name                    [ⓧ clear primaries?]
Add Info field              (○ Sentence)   ← Sentence only in Word; default OFF
Add Keywords or Attributes                 ← helper; mode-specific; hide Minimalist
[ Word ] [ Link ] [ Make ]                 ← selected = black; others = light grey
┌ Link results (Link mode only) ┐
└───────────────────────────────┘
Add to Favorites ›                    ★   ← Minimalist: star only
[ Show Hints … ]
Connections/Attributes                     ← header; hide Minimalist
  row… row…                                ← SEE §6.1
Contact                                    ← header; hide Minimalist
Phone field
Email field
[ Call ] [ Text ] [ Email ]                ← grey inactive / black active
General Info                               ← header; hide Minimalist
[ Tap to add notes…  › ]
[ Phone ] [ Demote|Promote ] [ Delete ]    ← Delete = red button
Date Created: …
Last Edited: …
You're doing great. Have an amazing day.
```

### Title row
- **Pencil** left of name — **contact masters only** — amends display name  
- Name: large bold  
- **Clear primaries** trailing `xmark.circle.fill` (secondary) — only if any primary attributes exist  

### Word / Link / Make
| Mode | Field placeholder | Helper (not Minimalist) | Does |
|------|-------------------|-------------------------|------|
| **Word** (default) | Add Info | Add Keywords or Attributes | Parse typed text into attributes / keywords; **Sentence** radio = treat as one literal sentence |
| **Link** | Search Existing Contacts | Link Existing Contact | Search existing; tap result to link |
| **Make** | Add Contact | Create New Contact | Create new contact-style node and link |

Selected button: **black** background, white label. Unselected: **systemGray5**, primary text.  
Symbols On: Word=`plus`, Link=`link`, Make=`hammer`.

### Sentence radio (Word only)
- To the **right** of Add Info  
- Off: empty `circle` (secondary)  
- On: `largecircle.fill.circle`  
- Default **off**; turns off when leaving Word  

### Favorites
- Row toggles favorite; filled star when on  

---

### 6.1 Connection / attribute rows (must match exactly)

**Header label:** `Connections/Attributes`  
**Empty:** `No connections` (no header)

#### Grip Hand = Right (default)

```
[ 🔴 xmark.circle.fill ]  [ Name ──────────────── ]  [ 🟢 plus.circle.fill ]  [ ≡ grip ]
```

#### Grip Hand = Left

```
[ ≡ grip ]  [ 🟢 plus.circle.fill ]  [ Name ──────────────── ]  [ 🔴 xmark.circle.fill ]
```

| Control | Symbol | Color / size | Action |
|---------|--------|--------------|--------|
| Disconnect | **`xmark.circle.fill`** | **Red** (filled circle — **not** a bare X) | Remove connection |
| Name | text | Leading, expands | Navigate to that Master/attribute |
| Add primary | **`plus.circle.fill`** | **Green**, ~`.title3` | Mark as primary attribute on this master; **hidden** if already primary |
| Reorder | **`line.3.horizontal`** | Secondary, ~30×44 hit target | Drag to reorder connections |

**Wrong Android patterns to fix:**
- Attributes list **above** Word/Link/Make  
- Red **bare X** without filled circle  
- Missing **green plus**  
- Missing **reorder grip**  
- Wrong left/right grip mirroring  

---

### Contact block
- **Phone** and **Email**: plain text (not blue links). Empty = placeholder field; saved = value + pencil to edit  
- **Call / Text / Email** three buttons under fields:  
  - Inactive: light grey fill  
  - Active: black fill, white text (phone dialable / email has `@`)  
  - Symbols On: `phone` / `message` / `envelope`  
- Long-press copy on values is enough; **no** dedicated copy icon  

### General Info
- One-line preview; tap → full notes editor; save on dismiss  

### Bottom action row
| Button | Symbols On | Style |
|--------|------------|--------|
| **Phone** (export / Add to Contacts) | `person.crop.circle.badge.plus` | Black |
| **Demote** (if contact master) / **Promote** (if attribute) | `arrow.down` / `arrow.up` | Black |
| **Delete** | `xmark` | **Red** fill, dark label |

Then date lines + encouragement.

---

## 7. List pages (from Home buttons)

| Page | Rows roughly |
|------|----------------|
| **Contacts** | Alphabetical / listed contact masters; tap → Master |
| **Recent** | Recently added |
| **Favorites** | Favorited nodes; may support reorder grip on favorites list (**without** red X / green +) |

Master connection chrome (X / + / grip) is **not** copied onto All Contacts rows.

---

## 8. Minimalist mode (quick reference)

**Hides:** helpers under fields, section headers (Connections/Attributes, Contact, General Info, Drafts/Notes title), Favorites label+chevron (star remains), Show Hints blocks.  
**Keeps:** fields, buttons, lists, results boxes, encouragement, chrome.

---

## 9. Symbols mode (quick reference)

When **Symbols** On, replace text on key black/grey button rows with icons (Home browse, Word/Link/Make, Call/Text/Email, bottom Master actions). When Off, show English (or localized) labels.

---

## 10. Behavior rules Android often misses

1. Title **"If I Must"** always returns Home  
2. Word is default Master mode  
3. Sentence only in Word; default off  
4. Link results sit **under** Word/Link/Make, **above** Favorites  
5. Primaries: green + adds; clear-all X on title when any primary exists  
6. Allow All typing in add/edit fields; `,` and `.` remain operators in parsers  
7. Production may gate behind subscription; Test bypasses  
8. Do not resurrect modifiers, chip-cloud connections, profile icon, or Quick Add as primary UX  

---

## 11. Android parity checklist (start here)

- [ ] Right chrome = **gear**, not profile  
- [ ] Menu has all items in §3 order  
- [ ] Settings has all items in §4 order  
- [ ] Home vertical order matches §5  
- [ ] Browse labels: **Contacts / Recent / Favorites**  
- [ ] Master vertical order matches §6 (**attributes below** Word/Link/Make)  
- [ ] Connection row: red **`xmark.circle.fill`**, name, green **`plus.circle.fill`**, **`line.3.horizontal` grip**  
- [ ] Grip Hand Left mirrors correctly  
- [ ] Call / Text / Email grey→black activation  
- [ ] Bottom Phone = person+plus badge icon when Symbols on  
- [ ] No modifier syntax UI  
- [ ] Minimalist / Symbols / Hints behave as above  

---

## 12. Related docs

| Doc | Role |
|-----|------|
| `WINDOWS-ANDROID-START.md` | How to start the Android agent |
| `ANDROID-HANDOFF.md` | Broader handoff / product notes |
| `NAME_MEMORY_APP_SPEC.md` | Product rules / graph behavior |
| `DESIGN_REFERENCE.md` | **Early** visual sketch — do not override this bible |
| `Design/screenshots/iphone/` | Optional current screenshots (add when available) |

---

*Generated from the shipping iPhone SwiftUI structure (ContentView, NodeDetailView, MainMenuViews, MasterContactFields, AppButtonSymbols, etc.). Update this file when iPhone UX changes.*
