# Flutter start guide — phased build (current iPhone parity)

**Goal:** One Flutter app (iOS + Android) that matches the **shipping iPhone** app.  
**UI bible:** `../Android-Parity/` (read `SOURCE-OF-TRUTH.md` + `DO-NOT-BUILD.md` first).

Work **phase by phase**. Do not skip to Master polish before Home + data model exist. Do not “improve” the design.

---

## Before Phase 0 (you on Mac)

- [ ] Xcode installed (for iOS Simulator)  
- [ ] Flutter SDK installed (`flutter doctor` clean enough to run)  
- [ ] Android toolchain optional on Mac (can use iOS Simulator first)  
- [ ] Cursor open on `Web Apps` or a dedicated Flutter project folder  
- [ ] Confirm `Android-Parity/` exists beside this folder  

Suggested project location (agent should confirm with you):

```text
/Users/randybevis/Web Apps/ifimust_flutter/
```

or sibling under Desktop — **not** inside the Xcode `If I Must` tree.

---

## Parity lock (read every phase)

Before coding any screen:

1. Open the matching `Android-Parity` doc + screenshot  
2. Skim `DO-NOT-BUILD.md`  
3. Implement **current** chrome only (gear, Word/Link/Make, list rows, etc.)

---

## Phase 0 — Tooling + empty app

**Done when:** `flutter run` shows a blank/scaffold “If I Must” on iOS Simulator (and ideally Android emulator).

1. Create Flutter project (Dart 3, Material 3 ok as base — restyle to match iPhone, don’t ship purple defaults).  
2. Package / bundle id: ask you to confirm (e.g. align with iOS `App.If-I-Must` / Android applicationId later).  
3. App name display: **If I Must**.  
4. Folder structure proposal: `lib/app`, `lib/features/home`, `lib/features/master`, `lib/data`, `lib/settings`, …  
5. Short note: how you’ll share code for iOS + Android.

**Skip:** paywall, CloudKit, full localization, Directories B2B.

---

## Phase 1 — Data model + local persistence

**Done when:** You can insert/query Masters, attributes, connections offline; data survives restart.

Model concepts (match iPhone):

- **Node** (Master vs attribute-capable; display name / label; dates; favorites; phone/email; general info; connection order; primaries)  
- **Connection** (bidirectional link between nodes)  

Suggested stack: **Drift** or **Isar** / sqflite — pick one and stick to it (see `STACK.md`).

Implement:

- Create contact from Add-style input (wire parser in Phase 2 if needed)  
- List all contact masters  
- Basic link between two nodes  

**Skip:** fancy UI; use debug screens if needed.

---

## Phase 2 — Home (current layout only)

**Bible:** `Android-Parity/02-HOME.md` + screenshots `01`–`03`.

**Done when:** Home matches vertical order and behaviors:

1. Nav: circular menu · **If I Must** · circular **gear** (not profile)  
2. Add Contact field + optional Show Hints  
3. Search + helper + grey results box  
4. **Contacts | Recent | Favorites** (or Symbols icons)  
5. Drafts/Notes row  
6. Encouragement line  

Wire Add + Search to the graph (parsers: spaces, `.` concatenate, `,` groups — from Specs / iPhone `AddInputParser` / search rules).

**Checklist extract:** no chip cloud; no modifiers; results box always present.

---

## Phase 3 — Navigation lists

**Bible:** `Android-Parity/06-LISTS.md` + screenshots `07`–`09`.

- All Contacts (A–Z + index)  
- Recent with **Past Day / Past Week / Past Month** (default Past Week)  
- Favorites with reorder grip (full-row drag later in Phase 5)

Tap → Master (Phase 4 stub ok).

---

## Phase 4 — Master page (highest risk of drift)

**Bible:** `Android-Parity/05-MASTER.md` + screenshots `10`–`12`.  
**Also:** `DO-NOT-BUILD.md`.

**Done when:** Vertical order is exact:

```
Title → Add Info (+ Sentence in Word) → helper → Word|Link|Make
→ Link results? → Favorites → hints? → Connections/Attributes BELOW
→ Contact → Call|Text|Email → General Info → bottom actions → dates → encouragement
```

Connection row (Grip Right):

`[red filled X] [blue name] [green filled + if not primary] [grip]`

No attributes-above-modes. No bare X. No missing green + / grip.

Word / Link / Make + Sentence mode behavior per bible + How-To.

---

## Phase 5 — Reorder + font scaling

**Bible:** `08-REORDER-DRAG.md`, `09-FONT-SCALING.md`.

- Drag grip → **entire row** floats (snapshot or full-row elevate), not grip alone  
- Font Size steps use **one global scale** (Small = 1.0); icons scale with text  

---

## Phase 6 — Menu + Settings + copy screens

**Bible:** `03-MENU.md`, `04-SETTINGS.md`, `HOW-TO-GUIDE.md`, `WHY-I-BUILT-THIS-APP.md`.

- Menu rows in order; Home blue, no chevron  
- How-To includes **Watch Tutorial Video** → `https://www.youtube.com/@IfIMustLLC`  
- Why I Built = exact 10 paragraphs  
- Settings: Hints, Symbols, Minimalist, Passcode, Theme, Grip Hand, Font Size, Delete Empty Containers, Backup  

---

## Phase 7 — Platform polish

- iOS: StoreKit-style subscription gate (mirror iPhone yearly + trial when ready)  
- Android: Play Billing (can follow after Windows native Android learnings)  
- JSON backup import/export  
- Light/Dark  
- Localization strategy later (iPhone has many languages — don’t block Phase 0–6)  
- Icons / splash / store listing assets  

---

## Phase 8 — Parity audit

Run `Android-Parity/07-ANDROID-CHECKLIST.md` as a Flutter checklist.  
Side-by-side: iPhone Simulator vs Flutter iOS.  
Fix drifts before calling “1.0”.

---

## What not to do in any phase

- Start from `DESIGN_REFERENCE.md`  
- “Simplify” Master by dropping green + or grip  
- Copy a wrong Android layout if it conflicts with Android-Parity  
- Build CloudKit / Directories / Black variant first  
- Redesign typography into a new brand look without asking  

---

## Suggested agent cadence

| Session | Focus |
|---------|--------|
| 1 | Phase 0 + 1 |
| 2 | Phase 2 Home |
| 3 | Phase 3 lists |
| 4–5 | Phase 4 Master |
| 6 | Phase 5 drag + font |
| 7 | Phase 6 menu/settings/copy |
| 8 | Phase 7–8 polish + audit |

After each phase: run on Simulator, compare to `Android-Parity/screenshots/`, then commit locally (only if you ask for a commit).
