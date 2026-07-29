# Android Parity Pack — If I Must (iPhone → Android)

**This folder is the single place the Windows Android agent should use for UI/UX parity.**

Repo path on GitHub: [`Android-Parity/`](https://github.com/rbevis333/ifimustdocs/tree/main/Android-Parity)  
Clone: `https://github.com/rbevis333/ifimustdocs.git`

Live iPhone code is **not** in this repo (Mac Desktop only). These docs + screenshots **are** the visual/functional source of truth for Android layout.

---

## What to tell the Windows agent (copy/paste)

```text
Open the folder Android-Parity/ and follow README.md reading order.

Match the current iPhone app exactly using:
1. Android-Parity docs (design + behavior)
2. Android-Parity/screenshots/*.png (visual target)

Do NOT rebuild early concepts from Design/DESIGN_REFERENCE.md (profile icon, chip clouds, modifiers).
Do NOT invent missing UI — if unsure, open the matching screenshot and the matching numbered markdown file.

Fix order: Home → Menu → Settings → Master → list pages (All Contacts / Recent / Favorites).
Work the checklist in 07-ANDROID-CHECKLIST.md before calling a screen “done.”

Also required when relevant:
- Reorder: 08-REORDER-DRAG.md (entire row must float — X, name, green +, grip — not grip alone)
- How-To: HOW-TO-GUIDE.md (include Watch Tutorial Video → https://www.youtube.com/@IfIMustLLC)
- Why I Built: WHY-I-BUILT-THIS-APP.md (exact 10 paragraphs)
```

---

## Reading order (required)

| # | File | Topic |
|---|------|--------|
| 0 | **This README** | How to use the pack |
| 1 | [`00-OVERVIEW-AND-EVOLUTION.md`](00-OVERVIEW-AND-EVOLUTION.md) | Product model + what was removed |
| 2 | [`01-VISUAL-SYSTEM.md`](01-VISUAL-SYSTEM.md) | Colors, radii, chrome, button states |
| 3 | [`02-HOME.md`](02-HOME.md) + screenshots `01`–`03` | Home layout & behavior |
| 4 | [`03-MENU.md`](03-MENU.md) + screenshot `04` | Hamburger menu |
| 5 | [`04-SETTINGS.md`](04-SETTINGS.md) + screenshots `05`–`06` | App Settings |
| 6 | [`05-MASTER.md`](05-MASTER.md) + screenshots `10`–`12` | Master page (most Android bugs) |
| 7 | [`06-LISTS.md`](06-LISTS.md) + screenshots `07`–`09` | All Contacts / Recent / Favorites |
| 8 | [`07-ANDROID-CHECKLIST.md`](07-ANDROID-CHECKLIST.md) | Pass/fail checklist |
| 9 | [`08-REORDER-DRAG.md`](08-REORDER-DRAG.md) | **Full-row** drag (not grip-only) — snapshot behavior |
| 10 | [`HOW-TO-GUIDE.md`](HOW-TO-GUIDE.md) | Full How-To text + YouTube link |
| 11 | [`WHY-I-BUILT-THIS-APP.md`](WHY-I-BUILT-THIS-APP.md) | Full Why I Built copy |

---

## Screenshot index

| File | Shows |
|------|--------|
| [`screenshots/01-home.png`](screenshots/01-home.png) | Home default (Hints on, text buttons) |
| [`screenshots/02-home-search-result.png`](screenshots/02-home-search-result.png) | Search query + result row + keyboard Search |
| [`screenshots/03-home-symbols-minimalist.png`](screenshots/03-home-symbols-minimalist.png) | Symbols + Minimalist on Home |
| [`screenshots/04-menu.png`](screenshots/04-menu.png) | Menu sheet |
| [`screenshots/05-settings-top.png`](screenshots/05-settings-top.png) | Settings Appearance + Text + Delete Empty |
| [`screenshots/06-settings-backup.png`](screenshots/06-settings-backup.png) | Settings Backup section |
| [`screenshots/07-all-contacts.png`](screenshots/07-all-contacts.png) | All Contacts A–Z |
| [`screenshots/08-recent-past-week.png`](screenshots/08-recent-past-week.png) | Recent list (Past Week title control) |
| [`screenshots/09-favorites.png`](screenshots/09-favorites.png) | Favorites with grip |
| [`screenshots/10-master-top.png`](screenshots/10-master-top.png) | Master top: title, Word/Link/Make, connections |
| [`screenshots/11-master-bottom.png`](screenshots/11-master-bottom.png) | Master Contact → Delete → dates |
| [`screenshots/12-master-symbols-minimalist.png`](screenshots/12-master-symbols-minimalist.png) | Master Symbols + Minimalist |

---

## Known Android mistakes these docs exist to fix

1. Connections/Attributes list placed **above** Word / Link / Make — wrong; must be **below**.
2. Connection row missing **green filled-circle plus** and/or **reorder grip**.
3. Disconnect control is a bare red X — wrong; must be **red `xmark.circle.fill`** (filled circle with X).
4. Right nav is a profile avatar — wrong; must be **gear**.
5. Early “modifiers” / chip-tag clouds rebuilt — wrong; removed on iPhone long ago.
6. Home button labeled “All” — UI label is **Contacts** (list title is **All Contacts**).
7. Recent screen title stuck as “Recent” — center control shows **Past Day / Past Week / Past Month** (default Past Week).

When screenshots and markdown disagree on a tiny pixel, prefer the **screenshot**. When behavior is unclear from a still image, prefer the **markdown** (written from shipping SwiftUI).
