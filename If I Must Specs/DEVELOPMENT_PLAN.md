# Name Memory App — Development Plan

Use this plan with the **NAME_MEMORY_APP_SPEC.md** in Cursor on your Mac. Open both files in the same project so the AI and you can reference the spec while building.

---

## Prerequisites

- [ ] Mac with Xcode installed (latest stable)
- [ ] Apple Developer account ($99/year) for device testing and App Store
- [ ] iOS 17+ target (SwiftData) or iOS 16+ (Core Data)
- [ ] Project folder in Cursor with spec + this plan

---

## Phase 1: Project Setup & Data Layer

**Goal:** New Xcode project with data models and local persistence.

| Step | Task | Notes |
|------|------|--------|
| 1.1 | Create new iOS App in Xcode (SwiftUI, SwiftData if iOS 17+) | No storyboards; SwiftUI lifecycle |
| 1.2 | Define **Node** model (label, baseLabel, numericSuffix, primaryAttributeId, isUnique, generalInfo, dateCreated) | See Spec §1 |
| 1.3 | Define **Connection** model (nodeA, nodeB, dateCreated) | See Spec §1 |
| 1.4 | Wire SwiftData/Core Data container; confirm save/fetch works | Local only first |
| 1.5 | Add input sanitization helper (trim, control chars, Unicode NFKC) | See Spec §11 |
| 1.6 | Add length limits (Search 500, Add/Edit 2000, node 100, General Info 500, 50 nodes per add) | See Spec §11 |

**Done when:** You can create and persist Node and Connection in code; sanitization and limits are in place.

---

## Phase 2: Add Screen & Input Parsing

**Goal:** Quick Add with comma/space/+ parsing and connection creation.

| Step | Task | Notes |
|------|------|--------|
| 2.1 | Build **Add** screen: single text field, Save button | Spec §3, §8 |
| 2.2 | Implement **parsing:** split by comma → groups; within group split by space; apply **+** concatenation (one space) | Spec §3 |
| 2.3 | First word in each group = master; create/find nodes; connect master to all other words in group | Spec §3 |
| 2.4 | Connect all masters in the same add to each other | Spec §3 |
| 2.5 | **Duplicate masters:** if master label exists, create "Label 2", "Label 3" (check isUnique) | Spec §4, §5 |
| 2.6 | Enforce max nodes per add (50) and max input length (2000) | Spec §11 |
| 2.7 | Navigate back to Home (or show success) after save | |

**Done when:** User can enter e.g. `Cody bald Texas + State garden, George old black dog, Kayla fiance` and see correct nodes and connections in the database.

---

## Phase 3: Home, Search & Results

**Goal:** Home screen with search and search results.

| Step | Task | Notes |
|------|------|--------|
| 3.1 | Build **Home** screen: search bar, Add button | Spec §2 |
| 3.2 | **Search:** single keyword → nodes whose label matches (or is connected to matching node); show connected nodes as results | Spec §7 |
| 3.3 | **Multi-keyword:** comma-separated AND logic — nodes connected to ALL keywords | Spec §7 |
| 3.4 | Apply search length limit (500) and sanitization | Spec §11 |
| 3.5 | **Search results list:** show node display name (primary attribute or "Label N" or label); tap → Node Detail | Spec §1 Display Rules |
| 3.6 | Empty state when no search or no results | |

**Done when:** User can search "dog", "dog, old", "dog, old, black" and get correct narrowing results; tapping a result opens Node Detail.

---

## Phase 4: Node Detail & Edit

**Goal:** View a node, its connections, Add Info, General Info.

| Step | Task | Notes |
|------|------|--------|
| 4.1 | Build **Node Detail** layout order: Connections (top) → Add Info field (middle) → General Info (bottom) | Spec §6 Node Detail Layout |
| 4.2 | **Connections:** list with red X to remove; tap name → navigate to that node’s detail | Not `d*` text modifier |
| 4.3 | **Add Info:** same parsing as Add but scoped to current node (no comma groups); words add connections; `.` concatenates | Spec §6 |
| 4.4 | **Primary attribute:** `...` prefix in Add Info (e.g. `... bald`); clear with `...` alone | Not `p*` modifier |
| 4.5 | **Display name:** show baseLabel + primary attribute label when set, else "Label N", else label | Spec §1 Display Rules |
| 4.6 | **General Info:** notes field; 500 char max; optional popup per spec | Spec §6 General Info |
| 4.7 | Sanitize and enforce length limits on Add Info and General Info | Spec §11 |

**Done when:** User can open Cody, see connections, add "truck cat" via Add Info, remove a connection with X, set primary with `... bald`, edit General Info, and see "Cody bald" when primary is set.

**Not in scope:** `d*`, `p*`, `u*` text modifiers (removed from spec).

---

## Phase 5: Menu, Quick Guide, App/Account Shell

**Goal:** Navigation and in-app help.

| Step | Task | Notes |
|------|------|--------|
| 5.1 | **Menu** (dropdown or slide-out): Navigation (Home, Add, recent searches), App Info, Account Info, Quick Guide | Spec §2 Menu |
| 5.2 | **Quick Guide** screen: content from Spec §9 (Add, Search, `.` concat, Add Info, `...` primary, General Info) | Spec §9 |
| 5.3 | **App Info:** version, credits, support link/email | |
| 5.4 | **Account Info:** placeholder for Settings (appearance, backup, sync later) | |

**Done when:** Menu opens, Quick Guide is readable, App Info and Account Info screens exist.

---

## Phase 6: Storage & Sync Options

**Goal:** Confirm local storage behavior and optional iCloud sync.

| Step | Task | Notes |
|------|------|--------|
| 6.1 | **Appearance:** Keep current minimal settings as-is (theme + font size step). No advanced appearance expansion. | Current behavior accepted |
| 6.2 | **Storage:** Ensure data is local only (SwiftData/Core Data in app container) | Spec: no server storage |
| 6.3 | **Optional iCloud:** Enable iCloud + CloudKit; configure SwiftData/CloudKit or NSPersistentCloudKitContainer so data syncs to user’s iCloud | User’s iCloud only; no backend |

**Done when:** Current appearance settings remain stable; data stays on device (and optionally syncs to their iCloud).

---

## Phase 7: Polish & App Store Prep

**Goal:** Ready for TestFlight and App Store.

| Step | Task | Notes |
|------|------|--------|
| 7.1 | App icon, launch screen | |
| 7.2 | Test on device; fix crashes and layout issues | |
| 7.3 | Privacy: no server collection; data on device / user iCloud; state in App Store Connect and privacy policy | |
| 7.4 | App Store Connect: create app, metadata, screenshots, privacy policy URL | |
| 7.5 | Archive, upload build, submit for review | |

---

## Order Summary

1. **Phase 1** — Data models + persistence + validation  
2. **Phase 2** — Add screen + full parsing + duplicate/unique logic  
3. **Phase 3** — Home + search + results  
4. **Phase 4** — Node Detail + Edit + General Info  
5. **Phase 5** — Menu + Quick Guide + App/Account screens  
6. **Phase 6** — Storage behavior + iCloud option  
7. **Phase 7** — Polish + App Store  

---

## Using This in Cursor on Mac

- Keep **NAME_MEMORY_APP_SPEC.md** and **DEVELOPMENT_PLAN.md** in the same folder as your Xcode project (or in a `docs` folder and reference them).
- In Cursor, open the project folder and @ mention the spec when asking for code: e.g. *"Implement the Add screen parsing per NAME_MEMORY_APP_SPEC.md section 3."*
- Work through phases in order; check off steps as you go.
