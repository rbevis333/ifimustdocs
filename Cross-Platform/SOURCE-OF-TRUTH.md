# Source of truth (priority order)

When Flutter docs, agent guesses, and screenshots disagree, use this order:

## 1. Current iPhone (behavior + pixel intent)

- App: `/Users/randybevis/Desktop/If I Must/`
- Scheme: **If I Must** (production) / **If I Must Test** (dev bypass paywall)
- For “what does this control do?” — read SwiftUI (`ContentView`, `NodeDetailView`, `MainMenuViews`, `ReorderGrip`, `AppFontSize`, etc.)

## 2. Android-Parity pack (layout bible + screenshots)

**Local:** `/Users/randybevis/Web Apps/Android-Parity/`  
**GitHub:** https://github.com/rbevis333/ifimustdocs/tree/main/Android-Parity

| File | Use for |
|------|---------|
| `00-OVERVIEW-AND-EVOLUTION.md` | Product model + evolution |
| `01-VISUAL-SYSTEM.md` | Colors, buttons, chrome |
| `02-HOME.md` … `06-LISTS.md` | Screen order & behavior |
| `07-ANDROID-CHECKLIST.md` | Pass/fail (reuse for Flutter) |
| `08-REORDER-DRAG.md` | Full-row drag snapshot |
| `09-FONT-SCALING.md` | Font size scales icons too |
| `HOW-TO-GUIDE.md` | Exact How-To + YouTube |
| `WHY-I-BUILT-THIS-APP.md` | Exact story copy |
| `screenshots/01`–`12` | Visual target |

This pack was written from **shipping** SwiftUI + **current** screenshots (July 2026). Prefer it over any older Specs sketch.

## 3. Product / parser specs (secondary)

`If I Must Specs/` — graph rules, NOTES, development history. Use for Add/Search parsing edge cases, not for inventing UI chrome.

## 4. Historical only (do not drive Flutter UI)

- `If I Must Design/DESIGN_REFERENCE.md`
- Early layout PNGs that show profile icon / chip clouds
- Old chat threads that propose modifiers or Quick Add as primary UX

## 5. Windows Android app

Useful as a **second implementation**, not as the design source. If Android disagrees with `Android-Parity` or iPhone, **Flutter follows iPhone / Android-Parity**, then fix Android later.

---

## Keeping this pack current

When iPhone UX changes:

1. Update `Android-Parity/` (screenshots + md) first  
2. Note the change in this folder’s `START-GUIDE.md` “Parity lock” section if phases are affected  
3. Push `ifimustdocs` so Windows + Mac stay aligned  

Do **not** let Flutter invent a third design language.
