# 00 — Overview & evolution

## What the app is

**If I Must** is a contact/attribute memory graph:

- A **Master** is usually a person (or contact-capable node) whose display name can include primary attributes (e.g. `lisa nurse blonde`).
- **Attributes** are nodes connected to Masters (e.g. `nurse`, `blonde`, `jim tall doctor`).
- Users **add**, **search**, **link**, **reorder**, mark **primaries**, store **phone/email/notes**, and browse **Contacts / Recent / Favorites**.

Android must match the **current shipping iPhone** behavior and layout — not early mockups.

---

## Evolution: early idea → now

### Early design (do **not** rebuild)

From old design PNGs / `DESIGN_REFERENCE.md`:

| Early idea | Status |
|------------|--------|
| Right header = circular **profile/avatar** | **Removed** → gear Settings |
| Connections as wrapping **tag chips** | **Removed** → ordered list rows |
| Modifier prefixes (`d*`, `p*`, `u*`) | **Never shipping** — do not add |
| Standalone Quick Add screen | **Orphaned** — Home inline Add replaced it |
| Phone/email as blue `tel:` / `mailto:` links | **Removed** → Call / Text / Email buttons |
| Copy icons beside phone/email | **Tried then removed** |

### Current chrome (match this)

| Position | Control |
|----------|---------|
| Left | Hamburger menu in a **circular white button** with light shadow |
| Center | Bold text **"If I Must"** — tap returns **Home** |
| Right | **Gear** in a circular white button — App Settings |
| On Master (when stack has history) | Also a **circular back chevron** left of the hamburger |

---

## Screen map

```
Paywall (production) → Home
Home
  ├─ Menu sheet
  ├─ App Settings sheet
  ├─ All Contacts
  ├─ Recently Added (Past Day / Week / Month)
  ├─ Favorites
  └─ Master (node detail)
        ├─ other Masters / attributes via connection taps
        └─ notes sheets, confirm dialogs
```

---

## Modes that change appearance everywhere

| Setting | Effect |
|---------|--------|
| **App Hints** On | Show “Show Hints” / expandable hint bullets where applicable |
| **Symbols** On | Key buttons show icons instead of text labels |
| **Minimalist** On | Hide helper labels, section headers, Favorites text (star remains), hint UI |
| **Grip Hand** Left/Right | Mirrors Master connection row controls |
| **Theme** Light/Dark | System appearance preference in app |

See `01-VISUAL-SYSTEM.md` and per-screen files for exact hide/show lists.

---

## Source priority for the Android agent

1. `Android-Parity/screenshots/` — how it **looks**
2. `Android-Parity/*.md` — how it **works** and exact order
3. Product specs in `Specs/` — graph rules / parsers (secondary for layout)
4. Early `Design/DESIGN_REFERENCE.md` — historical only
