# Windows — start Android If I Must (step by step)

**Audience:** You on the Windows PC + a **new Cursor agent** in a fresh chat.  
**Goal:** Clone docs, open a project, create the Android app, parity with iPhone behavior.  
**Already done:** Android Studio installed · Google Play Console business account approved · docs on GitHub.

**Repo:** https://github.com/rbevis333/ifimustdocs (private)

---

## What order to do things (short)

1. Clone **ifimustdocs** on Windows (Specs, Design, chat).  
2. Open that folder in Cursor → **new Agent** chat.  
3. Paste the **starter prompt** below (do **not** say only “look at all the files”).  
4. Let the agent propose an Android project layout + create the empty app in Android Studio / Gradle.  
5. Build & run a hello/skeleton on emulator or device.  
6. Then implement screen-by-screen parity (Home → Master → Search → paywall).

Do **not** manually copy Specs into a second random folder first — clone once, work from that clone (or open clone + create the Android project **beside** it or **inside** a subfolder the agent creates).

---

## Part A — You do this on Windows (before the agent codes)

### A1. Install / confirm tools

- [ ] **Git for Windows** (if not installed): https://git-scm.com/download/win  
- [ ] **Cursor** installed and signed in  
- [ ] **Android Studio** (you have this) — open once; finish SDK / first-run wizard if prompted  
- [ ] **JDK** — Android Studio’s embedded JDK is fine (usually JDK 17+)  
- [ ] Optional: GitHub account logged in (browser or Git Credential Manager) so private clone works  

### A2. Clone the docs repo

In PowerShell or Git Bash:

```powershell
cd $env:USERPROFILE\Documents
git clone https://github.com/rbevis333/ifimustdocs.git
cd ifimustdocs
```

If GitHub asks you to sign in, use **rbevis333** (private repo).

You should see:

- `If I Must Specs/` — plans, NOTES, **iPhone-App-Discussion-chat.txt**  
- `If I Must Design/` — layouts, icon assets  
- `ANDROID-HANDOFF.md`  
- `WINDOWS-ANDROID-START.md` (this file)  

### A3. Open Cursor correctly

1. Cursor → **File → Open Folder** → select `Documents\ifimustdocs` (the clone).  
2. Start a **New Agent** chat (do not continue an old Mac chat — those do not sync).  
3. Keep this file open or @-mention it: `@WINDOWS-ANDROID-START.md`

### A4. Starter prompt (paste into the new agent)

Copy everything in the box below:

```text
Read WINDOWS-ANDROID-START.md and ANDROID-HANDOFF.md first.

Then read once (for context only — do not re-implement from chat alone):
- If I Must Specs/iPhone-App-Discussion-chat.txt
- If I Must Specs/NAME_MEMORY_APP_SPEC.md
- If I Must Specs/DEVELOPMENT_PLAN.md
- If I Must Specs/If-I-Must-App-NOTES.md
- If I Must Design/DESIGN_REFERENCE.md

Goal: Build an exact functional copy of the iOS app "If I Must" for Android.
Stack: Kotlin + Jetpack Compose + Room + Play Billing (yearly + trial like iOS).
Live iPhone source is on Mac Desktop/If I Must — NOT in this repo. Specs/Design here are the handoff.

Phase 1 (do this first, before feature screens):
1. Confirm Android Studio / SDK assumptions.
2. Create a new Android app project (package suggestion: com.ifimust.app or app.ifimust — ask me to confirm).
3. Put the project in a clear folder (e.g. ifimustdocs/android/ or sibling folder Documents/IfIMustAndroid) and explain which.
4. Wire minSdk / targetSdk reasonably (API 26+ min unless you justify otherwise).
5. Get a blank app running on emulator or device.
6. Give me a short parity checklist (Home, Search, Master Word/Link/Make, Call/Text/Email, Drafts/Notes, Favorites, subscription gate).

Do not invent a redesign. Match iOS behavior and parsing rules (spaces / periods / commas; Word Sentence mode). Offline-first local DB; sync later if needed (no CloudKit on Android).
Ask before creating a new GitHub repo for the Android code.
```

---

## Part B — What the agent should do (Phase 1)

| Step | Outcome |
|------|---------|
| Read handoff + key Specs | Shared vocabulary (Master, attributes, Sentence mode, etc.) |
| Create Android Studio / Gradle project | Empty “If I Must” app |
| Run on emulator/device | Confirms toolchain |
| Propose folder + Git plan | Docs stay in `ifimustdocs`; **app code** may be a second repo later |
| Parity checklist | Roadmap for Phase 2+ |

**Skip for Phase 1:** Full feature parity, Play Billing wiring (can stub), localization of all 49 languages, Directories B2B.

---

## Part C — After Phase 1 works

Implement in roughly this order (agent can refine):

1. Data model (nodes, connections) + Room  
2. Home: Add Contact + Search  
3. Master page: Word / Link / Make + connections  
4. Phone / email fields + Call / Text / Email buttons  
5. Drafts/Notes + General Info  
6. All / Recent / Favorites  
7. Paywall + Play Billing  
8. Backup JSON import/export (match iOS if needed)  
9. Polish / Settings (Minimalist, symbols, etc.)

Use Design PNGs as layout reference, Specs for rules.

---

## Answers to common questions

### “Just say look at all the files?”

**No.** Point the agent at **this file** + the starter prompt. Indexing the whole chat dump alone wastes tokens; Specs + Design are the source of truth for rules/layout; the chat is **optional context once**.

### “Create similar files locally for Android?”

**Yes — but via the agent** as a real Android Studio project (Gradle), not by hand-copying Markdown into Kotlin. Keep Specs/Design in the cloned `ifimustdocs` folder.

### “Copy Specs to local computer?”

**Clone replaces copy.** One `git clone` is enough. Update later with `git pull`.

### “Where is the real iPhone Swift code?”

Still on the **Mac**: `Desktop/If I Must/`. It is **not** in `ifimustdocs`. For pixel-perfect or parser edge cases, either:

- Push/open that project in a separate GitHub repo later, or  
- Paste specific Swift files into the Windows chat when needed.

### Chat history from Mac Cursor?

**Does not appear on Windows.** That is why `iPhone-App-Discussion-chat.txt` is in this repo.

---

## Play Console (you already have an account)

When the skeleton runs:

1. Create an app listing in Play Console when ready (can wait until first internal test build).  
2. Application ID must match the Gradle `applicationId`.  
3. Billing / subscription product IDs — define later to mirror iOS yearly + trial.

No need to create the Play listing on day one if you only want emulator runs.

---

## Quick checklist (print / tick)

- [ ] Clone `rbevis333/ifimustdocs`  
- [ ] Open folder in Cursor  
- [ ] New Agent  
- [ ] Paste starter prompt from §A4  
- [ ] Agent creates Android project + empty app runs  
- [ ] Then start Home / graph parity  

---

*Last updated: July 2026*


## UI parity (required for layout fixes)

Read **`Specs/IPHONE_UI_PARITY.md`** before changing Android layout. It is the source of truth for Home/Master/Menu/Settings order and controls. Optional screenshots: `Design/screenshots/iphone/`.
