# 04 — App Settings

**Screenshots:** `05-settings-top.png`, `06-settings-backup.png`

Opened from gear **or** Menu → App Settings.

## Chrome

- Title: **App Settings**
- Trailing: **Done** (dismiss)
- Grouped list on light grey background, white rounded section cards

---

## Section: Appearance

Each row = label left + **segmented control** right (pill).

| Row | Options | Default (typical) | Effect |
|-----|---------|-------------------|--------|
| **App Hints** | On / Off | On | Show/hide Show Hints blocks |
| **Symbols** | On / Off | Off | Icons vs text on key buttons |
| **Minimalist** | On / Off | Off | Hide helpers & section headers |
| **Passcode** | On / Off | Off | App lock |
| **Theme** | Light / Dark | Light | Appearance |
| **Grip Hand** | Left / Right | Right | Master connection row mirroring |

---

## Section: Text

| Control | Detail |
|---------|--------|
| Font size | Circular **+** left, label center `Font Size (…)` e.g. `X Small`, circular **−** right |
| Footnote | `iPad 1 and iPad 2 are the largest sizes — intended for iPad screens.` |

+/- are bordered / blue-tint circular controls (iOS bordered style).

---

## Untitled cleanup section

1. Footnote (centered grey): explains removing orphan attribute-only nodes after disconnect; Contact Masters are kept.  
   Example wording: *Removes attribute-only nodes left behind after you disconnect them (e.g. “texan” with no links). Contact Masters such as “bbq.7/4/26” are not removed.*
2. Button **Delete Empty Containers**
   - Full width
   - **Red** background
   - **Black** text
   - Confirm before destructive work

---

## Section: Backup

| Row | Detail |
|-----|--------|
| Email field | Placeholder: `Your email (backup will be sent here)` |
| **Email JSON backup** | Blue action text — export JSON via mail/share |
| **Import JSON backup…** | Blue action text — import; can replace or merge |
| Footnote | Explains Mail / Simulator limits / merge vs replace |

See full footnote text in `06-settings-backup.png`.

---

## Test-only (not in production screenshots)

- Warning + **Delete All Contacts** (destructive) on Test builds only.

---

## Android parity traps

- [ ] Missing Grip Hand (needed for Master row mirroring)
- [ ] Missing Symbols or Minimalist
- [ ] Delete Empty Containers styled as blue text link instead of red bar
- [ ] Backup section missing
- [ ] Settings not reachable from both gear and Menu
