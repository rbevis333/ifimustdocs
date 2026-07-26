# If I Must — App notes (reference)

Planning and technical notes for the **consumer If I Must app** (not Directories B2B portal).

**Related:** `IF_I_MUST_CLOUDKIT_AND_TESTFLIGHT_CHECKLIST.md` (same Specs folder) · `../If-I-Must-Directories/NOTES.md`

**Todo priority (June 2026):** **Refresh Fix (P1)** after 1.1 approval → then Tier 1 / directories / lazy-load as scheduled

---

## Performance — stress test observation (pre–App Store submit)

**File:** `Web Apps/If-I-Must-Benchmarks/If-I-Must-stress-test-10000.json` (~**4.1 MB**, 4,274,060 bytes)

| | |
|--|--|
| Masters | 10,000 |
| Total nodes | ~10,026 |
| Connections | ~10,000 |
| General Info | none |

**Observed:** After import, the **app slowed noticeably** on device.

**Important:** **4 MB on disk ≠ 4 MB of runtime work.** JSON expands into thousands of SwiftData `Node` + `Connection` objects, relationship wiring, SQLite storage, and (on device) possible CloudKit sync—not a single flat 4 MB blob.

---

## Why large apps on the phone don’t feel slow (but this can)

Other apps may be **gigabytes** (video, maps, images, cache). That size is mostly **media**, loaded **lazily** and **paginated**.

If I Must at stress-test scale holds **tens of thousands of graph objects** the UI **queries and traverses** during search and browse—not the same problem as storing photos.

### Current bottlenecks (code paths)

1. **Home search** (`SearchService.search`) — `FetchDescriptor<Node>()` loads **all nodes** every search, then filters and walks connections in memory.
2. **All Contacts / Recent** — fetch all nodes, filter masters, sort in memory.
3. **Import** (`GraphImportExport.insertFreshGraph`) — bulk insert ~10k+ nodes, multiple saves, wire connections/primaries/order.
4. **Graph semantics** — search matches **start of any word** in a label and follows **connections**; more work than a simple name lookup.

**Designed for:** personal-scale graphs (dozens to low hundreds of contacts). **Not yet optimized for:** expo-scale imports in the **primary** graph.

---

## Search partitioning brainstorm — 28 buckets (A–Z, 0–9, merge)

**Hypothesis (Randy):** Split the graph into 28 searches (26 letters + numbers + merge)—would that speed search?

### Verdict

| Question | Answer |
|----------|--------|
| **Possible?** | Yes mechanically, but **not** as 28 full graph scans without changing behavior or adding indexes. |
| **Dumb idea?** | **No** — partitioning/sharding is valid; **first-letter-of-whole-label** is the wrong split for current rules. |
| **Faster as described?** | **Usually no** — often **slower** (28 fetches + merge overhead). |

### Why first-letter buckets don’t fit

Search rule: keyword must match **start of full label** OR **start of any word** in the label (e.g. query `bank` matches `john tall **bank**er`).

Bucket by **first character of entire label** puts `john tall banker` in **`j`**, but keyword `bank` belongs in word **`b`** → **miss** unless you still scan most of the graph.

Running **all 28 buckets every query** scans ~everything **28 times**. Running **one** bucket only works for “first letter of whole name” search—not current If I Must behavior.

### Better directions (preserve current search rules)

1. **Word / prefix index** — on save, index each word → node IDs; query `bank` hits index, then small graph expansion only from matches.
2. **SQL / SwiftData predicates + token table** — don’t `fetch(all nodes)`; filter in database.
3. **SQLite FTS5** (full-text search) — optional; needs schema work.
4. **Separate graph partitions** — primary vs directory (see Directories notes); search primary and directory scopes independently.

**Todo:** **Research Performance Speed** — prototype token index or predicate-based search; measure on device at 1k / 5k / 10k nodes.

---

## Refresh Fix (P1 — after 1.1 approval)

**Symptom (1.0 Production, likely 1.1 too):**
- Home/Master “flash” on launch
- Returning from background resets to Home (navigation lost)
- **Red X on a connection/attribute** flashes to Home — should **stay on the current Master**
- Full **Master delete** → Home is correct (1.1 intentional via `resetToHomeAfterDelete`)

**Cause (navigation / resume):** `SubscriptionManager.refresh()` sets `accessState = .loading` on every `scenePhase == .active`, which unmounts `ContentView` and destroys `NavigationStack` / `AppRouter.path`. Test app unaffected (paywall bypassed). Can also fire after Face ID sheet when removing a connection.

**Cause (red X — code check):** `deleteConnection(between:and:)` does **not** call `resetToHomeAfterDelete()` — only refreshes `connectedNodes`. Flash to Home on attribute remove is almost certainly the same refresh remount, not intentional navigation.

**Fix direction:**
1. On foreground refresh, do **not** tear down UI if already `.subscribed` — revalidate entitlements in background; only show paywall if access revoked.
2. After (1), verify red X and Master page stay put; connection delete must never reset `NavigationPath`.
3. Call `invalidatePerformanceCaches()` on connection delete if Tier 1 enabled (when graph edits affect cache).

**Verify on:** Production — open Master → red X remove attribute → stay on Master; background → foreground → stay on Master.

---

## Search cache fix (P1 — build 1.2)

**Symptom (Jul 2026, Production with Tier 1 on):**
- Add contact on Home → search returns nothing
- Force-quit and reopen → contact appears in search
- Multiple users affected

**Cause:** Tier 1 `GraphPerformanceCache.ensureSearchCache` — when node/connection count changed after add, it rebuilt the search snapshot from **stale in-memory `cachedNodes`** instead of refetching from SwiftData. Restart cleared the cache → search worked. **Not an Apple OS change.**

**Fix:** Only use the edit-only fast path when node + connection counts are unchanged; refetch from DB when structure changes.

**Verify:** Home → add `testname tall` → search `testname` → result appears immediately (no restart).

---

## Notes digits fix (build 1.2)

**Symptom:** Numbers stripped in General Info, Home Drafts/Notes (and anywhere using `filterGeneralInfoLive`).

**Cause:** Unicode 15+ marks ASCII `0`–`9` as `isEmoji`; notes filter blocked them. Add/Edit was unaffected (`char.isNumber` allowed). Likely surfaced with iOS 26 / newer SDK — not user error.

**Fix:** `isEmojiCharacter` exempts ASCII digits. Superseded by **Allow all characters** below when `restrictAddEditCharacters = false`.

---

## Allow all characters (build 1.2)

**Default:** `AppBuildConfig.restrictAddEditCharacters = false` — all printable text in Add/Edit and notes (control chars stripped). Commas, spaces, and periods still have parser meaning only.

**Revert:** Set `restrictAddEditCharacters = true` in `AppBuildConfig.swift` — restores allowlist on Add/Edit, emoji filter on notes, and blocked-character warning under Add fields. Warning string kept in `en.lproj` only (for revert).

**Removed:** How-To “Allowed characters…” line; live warning when unrestricted. Dead l10n keys removed from all `.lproj` files.

---

## Offline launch (build 1.2)

**Symptom:** App stuck on “Loading…” with no cell service; Drafts/Notes unreachable.

**Cause:** `SubscriptionManager.refresh()` awaited `Product.products()` (network) before checking entitlements. Contacts/graph are already local (SwiftData + UserDefaults); paywall gate blocked the UI.

**Fix:** Restore last verified entitlement from UserDefaults on launch; check StoreKit entitlements before product fetch; load product in background when already subscribed.

**Note:** Full Apple Notes–style offline is already true for data once the app opens. Remaining gap: brand-new install with no prior entitlement still needs network once to subscribe.

---

## Word Sentence checkbox (experiment — Master page)

**Where:** Master → **Word** mode only — checkbox to the **right** of Add Info. Unchecked by default; clears when leaving Word.

**When checked:** `EditInputParser.execute(..., asLiteralSentence: true)` — whole field becomes **one** attribute. Spaces, periods, and commas are literal (no word-split / `.` concatenate). Still lowercased; still capped at **100** chars (`nodeLabel`).

**How-To / hints:** Explains the Sentence radio treats text naturally. Localized in all `.lproj` files (Jul 2026).

**Why:** Easier list/sentence-style attributes without using General Info / Drafts. Trial — may remove if unused.

**Revert:** Remove checkbox UI + `asLiteralSentence` path in `EditInputParser` / `NodeDetailView`.

---

## Quick update (next submit — after build 37)

| Item | Status |
|------|--------|
| **Search after add (Tier 1)** | Fixed in `GraphPerformanceCache.ensureSearchCache` — ship next build. |
| **Allow all characters** | `restrictAddEditCharacters = false` — ship next build. |
| **General Info + Drafts/Notes editors** | Keyboard inset + bottom scroll buffer; save on swipe dismiss (same as X). In code — ship next build. |
| *(add more small tweaks from testing today)* | |

**Submit:** bump build only (e.g. **1.2 build 38**). What’s New: search fix + editor improvements.

---

**Context:** Version **1.0 (35)** rejected (Guideline **2.3.2** metadata / paid content disclosure; **3.1.2(c)** subscription Terms in metadata). Resubmit focuses on **English ASC fixes** + **binary** with post-approval UI and navigation fix. **Tier 1** and **multi-language ASC copy** may slip to a later build.

---

### In this build — binary (Production **If I Must**)

| Item | Notes |
|------|--------|
| **Search/delete crash fix** | Clear Home search when opening a search result; after delete → clean Home (`NavigationPath` reset + clear search). Verified on **If I Must Test**. |
| Delete All removed from production | Test app only |
| **Minimalist** mode | App Settings; hides section headers + hints on Home/Master |
| Expanded font sizes | X Small through iPad 2 |
| Responsive encouragement footer | 75% width, single-line fit-to-width |
| **General Info** max **1,000** chars | Was 500; per-contact notes on Master |
| **Home Drafts/Notes** | Below All/Recent/Favorites; tap row → editor; max **5,000** chars; device-only scratch pad (not in graph backup or search) |
| Master UI polish | Phone placeholder “Phone”; hint copy; Hints/Minimalist behavior; **Master title copyable** (long-press to select) |
| Paywall visible on Simulator | DEBUG simulator paywall bypass removed (Production) — for paywall screenshot |
| If I Must Test + Benchmarks | Dev target only — not submitted |

---

### In this build — App Store Connect (English first)

| Item | Notes |
|------|--------|
| **Description** | Subscription disclosure + trial/price/auto-renew + **Privacy Policy** and **Terms of Use** links at end (English) |
| **Promotional text** | Clarify 2-week trial, then subscription required (English) |
| **Screenshots** | Include at least one **paywall** shot and/or subscription-required context |
| **App Privacy** | Privacy policy URL (Trust & Safety → App Privacy) — already set |
| **Terms** | Apple standard EULA + Terms link in Description — **no custom EULA required** |
| **App Review reply** | Note paywall + in-app subscription disclosures; do **not** claim “all localizations” until true |
| **What’s New** | Randy updates per release — not translated |
| **App Name** | Not localized |

**Do not translate amended Description/Promotional into other languages until English passes second review.** Then translate subscription footer + promotional for each ASC locale (see `If I Must Localization/` todos).

---

### In Xcode already — optional for this release

| Item | Notes |
|------|--------|
| **49 in-app locales + `en`** | `.lproj` files loaded; device language drives in-app UI. **OK to ship** without translating ASC listing per language. |
| **ASC listing copy (49 langs)** | Original Subtitle/Description/Keywords/Promotional in `app-store-connect/ASC-Copy-All-Languages.txt` — **hold**; paste after English subscription text is approved |

---

## Master contact actions (UI)

**Contact export button symbol:** `person.crop.circle.badge.plus` (was `phone`).

**Phone/email:** Plain text + pencil (no tappable links). **Call / Text / Email** row under email (same size as Word/Link/Make): grey/`systemGray5` when missing data; black when actionable (`tel:` / `sms:` / `mailto:`).

**Copy icon:** Tried and removed — long-press text selection remains.

**Next after these Apple UI changes:** Mac file org → Windows Android layout/sync → initial Android setup.

---

### Deferred from this build (may slip)

| Item | Notes |
|------|--------|
| **AFTER current Apple UI changes** | **1)** Mac Web Apps folders — **done** (`If I Must Specs` / `Design` / Localization). **2)** Plan Windows Android project layout + sync with Mac. **3)** Initial Android app setup. |
| **Ignore system Dynamic Type** | Parents use iPhone **Settings → Larger Text**; app should use **Menu → Text** only. Root already sets `.environment(\.sizeCategory, …)` — verify leaks (alerts, paywall, keyboard); consider `.dynamicTypeSize` cap + remap `AppFontSize` steps from a smaller baseline. **No code yet.** |
| **Tier 1 performance** | Code in repo; `AppBuildConfig.performanceTier1Enabled = false` — enable for Production when ready |
| **Tier 2** | Shelved — do not ship |
| **Localized ASC subscription footer** | After English resubmit approved |
| **Localized ASC promotional text** | After English resubmit approved |
| **Directories** | Later |
| **Lazy-load General Info** | Later |

---

### Performance tiers (reference)

| Tier | Status | Notes |
|------|--------|-------|
| **Tier 1** | **Off** in Production and Test | ~48 ms search at 10k in Test when enabled. Candidate for a future build. |
| **Tier 2** | **Off** — shelved | No measurable gain alone or with Tier 1 |

**Do not** change search rules or SwiftData schema for CloudKit unless a future tier explicitly requires it.

---

## Next build plan — Performance Tier 1 or Tier 2 (archive)

| Tier | What | Schema change? | Risk |
|------|------|----------------|------|
| **Tier 1** | Cache node fetch during a search session; run search off main thread; debounce tuning | **No** | Low — same search results |
| **Tier 2** | **In-memory word token index** rebuilt from existing nodes; index narrows candidates, then existing graph match logic | **No** (RAM-only index) | Low–moderate — must verify identical results vs today |

**Not in this release:** primary vs directory partition, FTS5, persistent token schema (Tier 3+).

**Order of work:** Tier 1 first (quick win), then Tier 2 if time/testing allows in the same build. Re-test on device with normal contacts + optional stress-test JSON import.

**Do not** change search rules or SwiftData schema for CloudKit unless Tier 2 proves insufficient and we explicitly plan Tier 3 later.

---

## Research Performance Speed — main app (checklist)

**Deferred until after Languages.** Tier 1 + Tier 2 prototyped in Test app; see benchmark section below.

When implementing / measuring:

- [ ] **Tier 1:** Cache fetches during search session; background search; debounce
- [ ] **Tier 2:** In-memory token index; candidate narrowing; verify same results as today
- [ ] Profile: time for `FetchDescriptor<Node>()` at 10k nodes
- [ ] Profile: single-keyword search + connection traversal cost
- [ ] Evaluate **word token index** (maintained on insert/edit/delete)
- [ ] Evaluate **FTS5** vs custom token table in SwiftData/SQLite
- [ ] Stop loading all nodes on every keystroke (debounce alone is insufficient)
- [ ] **All Contacts / Recent:** cache master list when graph unchanged since last open (see baseline UX note below)
- [ ] Import: background chunk insert + progress UI for large backups
- [ ] Re-test with `If-I-Must-Benchmarks/If-I-Must-stress-test-10000.json` after changes
- [ ] Document acceptable max nodes for **primary** graph without directory partition

---

## Benchmark baseline (Test app — Randy, June 2026)

**App:** If I Must Test · **JSON:** `If-I-Must-Benchmarks/If-I-Must-stress-test-10000.json` · **Graph:** 10,026 nodes after import

| Operation | Query / detail | Result | Timings |
|-----------|----------------|--------|---------|
| **Import** (replace all) | stress-test-10000 | 10,026 nodes | **6.38 s** |
| **Search all** (Home rules) | `"1021"` | 1 hit | **402, 409, 424, 422 ms** (4 runs, ~**414 ms** avg) |
| **Search masters** (Link rules) | `"1021"` | 1 hit | **394, 401, 414, 410 ms** (4 runs, ~**405 ms** avg) |

### 1k masters + 10 attrs + 250-char notes (Test app — Randy, June 2026)

**JSON:** `If-I-Must-Benchmarks/If-I-Must-benchmark-1000-masters-10-attrs-250-notes.json` · **Graph:** 11,000 nodes (1,000 masters, 10,000 attributes) · **Tiers off**

| Operation | Query / detail | Result | Timings |
|-----------|----------------|--------|---------|
| **Import** (replace all) | 1000-masters-10-attrs-250-notes | 11,000 nodes | **5.91 s** |
| **Search all** (Home rules) | `"m1"` | 11 hits | **446, 449, 454, 447 ms** (~**449 ms** avg) |
| **Search masters** (Link rules) | `"m1"` | 1 hit | **431, 445, 453, 450 ms** (~**445 ms** avg) |

Search time in line with the 10k file (~414 ms) despite fewer masters — extra attribute nodes, larger backup, and General Info on every master dominate. Import slightly faster (5.91 s vs 6.38 s).

### After Tier 1 (Test app — Randy, June 2026)

Same JSON, same device class:

| Operation | Timings |
|-----------|---------|
| **Import** | **6.05 s** |
| **Search all** `"1021"` | **56, 44, 48, 45 ms** (~**48 ms** avg) |
| **Search masters** `"1021"` | **49, 50, 50, 50 ms** (~**50 ms** avg) |

~**8× faster** search vs pre–Tier 1 baseline. All Contacts opens instantly on first visit (cached masters).

**Recents empty after stress import:** `If-I-Must-stress-test-10000.json` sets every node's `dateCreated` to **2026-01-01** (fixed at export). Recent filters (Past Day / Week / Month) only show masters created in that window — in June 2026, none qualify. Expected for this file; not a Tier 1 regression.

**All Contacts lag after favorite + back (fixed in Test):** Favoriting called `markContentEdited`, which bumped `dateLastEdited` and invalidated the full graph cache; `.task` refetched 10k nodes on return. Browse cache now ignores edit-only changes; All Contacts skips reload when browse fingerprint unchanged.

### Tier 2 alone (Test app — Randy, June 2026)

Tier 1 off, Tier 2 on. Search path still fetches all nodes; no browse cache.

| Operation | Timings |
|-----------|---------|
| **Import** | **6.04 s** |
| **Search all** `"1021"` | **522**, 397, 385, 383 ms (~**397 ms** avg excl. outlier) |
| **Search masters** `"1021"` | **370, 374, 388, 390 ms** (~**381 ms** avg) |

No meaningful improvement vs baseline (~414 ms). All Contacts / back navigation **4–5 s** (unchanged).

### Tier 1 + Tier 2 (Test app — Randy, June 2026)

Both on. Same as Tier 1 alone within variance.

| Operation | Timings |
|-----------|---------|
| **Import** | **6.12 s** |
| **Search all** `"1021"` | **55, 42, 55, 42 ms** (~**49 ms** avg) |
| **Search masters** `"1021"` | **45, 45, 45, 45 ms** (~**45 ms** avg) |

**Verdict:** Ship **Tier 1** when prioritizing performance post-Languages. **Do not ship Tier 2.**

**Re-test:** Menu → Benchmarks after each performance change; compare log + subjective All/Recent open time.

**Toggles (Test app):** `AppBuildConfig.performanceTier1Enabled` · `performanceTier2Enabled` — both **`false`** for normal use / baseline benchmarks. Flag matrix in `AppBuildConfig.swift`.

**Tier 1 (Test app):** `GraphPerformanceCache` + `SearchSnapshotEngine` + `GraphPerformanceTier1`.

**Tier 2 (Test app):** `SearchTokenIndex` / `SearchWordTokenIndex` — shelved.

**Benchmark JSON folder:** `Web Apps/If-I-Must-Benchmarks/` — see `NOTES.md` there.

---

## If I Must Test (development target)

Second app on the same phone for **development and testing** — separate sandbox and **separate iCloud container** from production **If I Must**.

| | Production | Test |
|--|------------|------|
| **Scheme** | If I Must | **If I Must Test** |
| **Display name** | If I Must | If I Must Test |
| **Bundle ID** | `App.If-I-Must` | `App.IfIMust.Test` |
| **CloudKit container** | `iCloud.App.If-I-Must` | **`iCloud.App.IfIMust.Test`** |
| **CloudKit on device** | Yes | **Yes** (same SwiftData path as prod) |
| **CloudKit on Simulator** | No (local) | No (local) |
| **Paywall** | Yes (Release + device) | **Bypassed** (`IF_I_MUST_TEST`) |
| **Ship to App Store** | Yes | **No** — dev-only |

**Build flag:** `IF_I_MUST_TEST` on the test target only (see `AppBuildConfig.swift`, `RootView.swift` `PaywallGate`, `If_I_MustApp.swift` `ModelContainerProviderCloudKit`).

**Workflow:** Run **If I Must Test** for imports, performance experiments, UI tweaks. Archive and submit only **If I Must**. Both apps can be installed side-by-side.

### Apple Developer setup — Test app (one time)

**You do NOT need a new App in App Store Connect.** Only **Certificates, Identifiers & Profiles** (Developer portal).

1. Go to [developer.apple.com](https://developer.apple.com) → **Account** → **Certificates, Identifiers & Profiles**.
2. **Identifiers → +** (or edit existing **`App.IfIMust.Test`**).
   - Type: **App IDs** → **App**
   - Bundle ID: **`App.IfIMust.Test`** (Explicit)
3. **Capabilities** — enable **iCloud**:
   - Check **CloudKit**
   - Under containers, click **+** or **Edit** and create/select: **`iCloud.App.IfIMust.Test`**
   - Do **not** attach `iCloud.App.If-I-Must` to the test App ID
4. **Save** the App ID.
5. (Optional) **Identifiers → iCloud Containers** — confirm **`iCloud.App.IfIMust.Test`** exists (often auto-created in step 3).
6. In **Xcode** → target **If I Must Test** → **Signing & Capabilities**:
   - Team: your team
   - Bundle ID: `App.IfIMust.Test`
   - Confirm **iCloud** + **CloudKit** + container **`iCloud.App.IfIMust.Test`** (should match `If_I_Must_Test.entitlements`)
7. On the **physical device**: **Settings → Apple ID → iCloud → iCloud Drive ON**.
8. Run scheme **If I Must Test** to the device (not Simulator — Simulator stays local-only, same as production).

**CloudKit Dashboard:** [icloud.developer.apple.com](https://icloud.developer.apple.com) → select container **`iCloud.App.IfIMust.Test`**. SwiftData usually deploys schema on first device launch (same as production).

**Note:** If you previously ran Test app with local-only `LocalTest` data, that data does **not** migrate to CloudKit — expect a fresh graph after enabling sync.

---

## Related files

- This Specs folder: plans, checklist, NOTES
- Design: `../If I Must Design/`
- Localization: `../If I Must Localization/`
- Search: `Desktop/If I Must/If I Must/SearchService.swift`
- Import/export: `Desktop/If I Must/If I Must/GraphImportExport.swift`
- Benchmark JSONs: `Web Apps/If-I-Must-Benchmarks/`
- Directory benchmark: `../If-I-Must-Directories/If-I-Must-directory-benchmark-1000-vendors.json`
- Folder map: `../README.md`

---

*Last updated: July 2026*
