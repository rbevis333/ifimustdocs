# If I Must — CloudKit + TestFlight Checklist (Novice-Friendly)

Use this in order. You already completed the hard portal setup (Identifier, iCloud container, App record in App Store Connect). This list picks up from there.

**Your app IDs (must match everywhere):**
- Bundle ID: `App.If-I-Must`
- iCloud container: `iCloud.App.If-I-Must`
- Xcode project folder: `Desktop/If I Must/`

---

## Before you start (one-time requirements)

- [ ] **Paid Apple Developer Program** membership is active (about $99/year). TestFlight and App Store need this.
- [ ] **Mac with Xcode** (you have this).
- [ ] **Real iPhone** for final CloudKit testing (your app code skips CloudKit in the Simulator on purpose).
- [ ] **Privacy policy URL** on the web (your `ifimust` site can host a simple privacy page — App Store will ask for a link).
- [ ] **Support email** ready: `info@ifimust.com` (you already use this).

---

## Part A — CloudKit (sync data to the user’s iCloud)

### A1. Confirm Apple Developer website (you mostly did this)

1. Go to [developer.apple.com](https://developer.apple.com) → **Account** → **Certificates, Identifiers & Profiles**.
2. Open **Identifiers** → select **`App.If-I-Must`**.
3. Confirm these are **enabled**:
   - **iCloud**
   - **CloudKit** (with container **`iCloud.App.If-I-Must`** selected)
4. Click **Save** if you change anything.

*Plain English:* Apple’s website must say this app is allowed to use your iCloud box.

---

### A2. Confirm App Store Connect app record

1. Go to [appstoreconnect.apple.com](https://appstoreconnect.apple.com) → **Apps** → **If I Must** (or your app name).
2. Confirm the app is tied to bundle ID **`App.If-I-Must`**.
3. You do **not** need a separate “CloudKit step” in App Store Connect for sync to work — CloudKit is mainly Developer portal + Xcode + code.

---

### A3. Xcode — Signing (so the app can run on your phone)

1. Open **`If I Must.xcodeproj`** in Xcode.
2. Click the blue **If I Must** project → select the **If I Must** target (under TARGETS).
3. Open **Signing & Capabilities**.
4. Check **Automatically manage signing**.
5. Choose your **Team** (your Apple Developer account).
6. Confirm **Bundle Identifier** is exactly: `App.If-I-Must`.

*Plain English:* Xcode needs permission to install the app on your iPhone under your developer account.

---

### A4. Xcode — Turn on iCloud capability (UI)

1. Still on **Signing & Capabilities** for the **If I Must** target.
2. Click **+ Capability** → add **iCloud**.
3. Under iCloud, enable **CloudKit**.
4. Select container **`iCloud.App.If-I-Must`** (create/select if prompted).

*This should match your entitlements file. If Xcode and the file disagree, use what matches Developer portal.*

---

### A5. Xcode — Entitlements file (project file)

**Status: done in code** (CloudKit re-enabled May 2026).

`If I Must/If_I_Must.entitlements` is active with `iCloud.App.If-I-Must` and CloudKit. **Code Signing Entitlements** is set for Debug and Release in the Xcode project.

If you clone on a new Mac, confirm **Build Settings → Code Signing Entitlements** = `If I Must/If_I_Must.entitlements`.

---

### A6. Xcode — Turn CloudKit on in app code

**Status: done in code.** `If_I_MustApp.swift` uses `ModelContainerProviderCloudKit.make()`. App Info mentions iCloud sync.

---

### A7. Build and test CloudKit on a real iPhone

**Status: done** (tested on physical iPhone).

1. Plug in your iPhone → select it as the run destination in Xcode (**Devices**, not Simulators — e.g. **R Cell**).
2. **Product → Clean Build Folder** (⇧⌘K), then **Run**.
3. On the phone: **Settings → Apple ID → iCloud → iCloud Drive ON** (and enough iCloud space).
4. If the app crashed after enabling CloudKit: **delete the app from the phone** and run again (schema change).
5. Add a test contact → force-quit → reopen and confirm data remains.
6. Optional: second device, same Apple ID — data should sync after a few minutes.

**Important:** Simulator stays **local-only** by design. That is normal.

---

### A8. If CloudKit fails (common fixes)

- Delete the app from the phone and install again (fresh database).
- Confirm container name matches everywhere: `iCloud.App.If-I-Must`.
- Confirm you are on a **physical device**, same Apple ID, iCloud Drive on.
- In Xcode: remove old iCloud capability and re-add it once.
- If you see a crash about “model container” / schema: tell your assistant — may need a one-time store reset (you hit this before when schema changed).

---

## Part B — TestFlight (install the app without the public App Store)

Do these **in order** (B1 → B10).

---

### B1. App icon (required before upload) — **done for now** (may revise later)

Apple rejects or blocks uploads without a valid app icon.

1. In Xcode, open **If I Must → Assets.xcassets → AppIcon**.
2. You need at least one **1024×1024** PNG (no transparency for App Store icon).
   - Modern template: drag the image onto the **1024×1024** slot (Universal, iOS).
   - Optional: separate **Dark** and **Tinted** 1024 assets if you want those appearances.
3. Confirm the icon appears in the slot (not an empty placeholder).

*Plain English:* This is the square icon on the home screen and in TestFlight.

---

### B2. iPhone screenshots (required for App Store Connect listing) — **done**

Capture **after** the app looks the way you want (icon, fonts, home screen). Do not skip this step.

**What to capture (minimum to start):**

| Device class | Size (pixels) | How to get them |
|--------------|-----------------|-----------------|
| **6.7" iPhone** (e.g. 15 Pro Max) | 1290 × 2796 | Run app on that size **Simulator**, **⌘S** to save screenshot to Desktop |
| **6.5" iPhone** (e.g. 11 Pro Max) | 1284 × 2778 | Second Simulator device if Apple requires both sizes |

**Suggested screens (3–5 images):**

1. **Home** — Add Contact + Search + gray results (type a letter so results show).
2. **Node detail** — a contact with connections and General Info visible.
3. **All Contacts** list.
4. **App Settings** — Backup section.
5. **Quick Guide** (optional).

**Upload in App Store Connect (step B5):**

1. [appstoreconnect.apple.com](https://appstoreconnect.apple.com) → **Apps** → **If I Must** → **App Store** (or **Prepare for Submission**).
2. Under **Screenshots**, select **iPhone 6.7" Display** (and 6.5" if required).
3. Drag PNG files into the slots.

---

### B3. Version and build numbers — **done** (Version **1.0**, Build **1**)

1. In Xcode → **If I Must** target → **General**:
   - **Version** (marketing): **`1.0`**
   - **Build**: **`1`** (increase to `2`, `3`, … on each **re-upload** to App Store Connect)

*Plain English:* Every upload needs a **new build number**, even if Version stays `1.0`. First TestFlight upload uses Build **1**.

---

### B4. App Store Connect — Required app information

Do **B4.1 → B4.4** in order (one step at a time). **Not needed for StoreKit** — subscription comes later (Part C, after TestFlight).

In App Store Connect → your app:

#### B4.1 — App Information (category + age rating) — **done**

1. Left sidebar → **App Information**.
2. Set **Primary category** (e.g. Social Networking or Lifestyle).
3. Click **Edit** next to **Age Rating** → answer the questionnaire → save.
4. Click **Save** on the page.

*Plain English:* Tell Apple what kind of app this is and who it’s appropriate for. No pricing here yet.

#### B4.2 — Privacy Policy URL — **done**

1. Still under **App Information** (or **App Privacy**), find **Privacy Policy URL**.
2. Enter your live link, e.g. `https://ifimust.com/privacy.html`.
3. Open that URL in a browser to confirm it works → **Save**.

#### B4.3 — App Privacy (nutrition label) — **done**

1. Left sidebar → **App Privacy** → **Get Started** or **Edit**.
2. **Collect data?** Yes. **Tracking?** No.
3. Add: Name, Phone, Email, Other user content — all for **App functionality**, linked to user, not for tracking.
4. **Publish**.

*Plain English:* The label users see on the App Store about data. Matches your privacy.html.

#### B4.4 — Pricing and Availability (download price only) — **done**

1. Left sidebar → **Pricing and Availability**.
2. Set app **download price** to **Free** (subscription $4.99/year is set up later in Part C, not here).
3. Leave territories as **all** (or pick countries) → **Save**.

*Plain English:* The app is free to **download**; you’ll charge via subscription before the public App Store launch.

**B4 complete.**

---

### B5. Listing text and screenshots upload

Do **B5.1 → B5.4** in order.

#### B5.1 — Open the version page + Support URL — **done**

1. App Store Connect → **If I Must** → **App Store** tab (or **Distribution** → **iOS App** → version **1.0** / **Prepare for Submission**).
2. Scroll to **App Store Listing** (or the version’s listing section).
3. **Support URL** (required): your live homepage, e.g. `https://ifimust.com/` — must show contact info (your site’s contact form + `info@ifimust.com` counts).
4. **Marketing URL** (optional): same as homepage, or leave blank.
5. **Save**.

*Plain English:* Apple does **not** ask for a separate “Support email” on the listing — only a **Support URL** that includes a way to contact you.

*Later (TestFlight for friends):* **TestFlight** tab → **Test Information** → **Feedback Email** = `info@ifimust.com` (at B9).

#### B5.2 — Subtitle and description ← **you are here**

#### B5.2 — Subtitle and description — **done**

- **Name:** If I Must (usually already set)
- **Subtitle:** short tagline (30 characters max), e.g. “Remember people you meet”
- **Description:** a few sentences about what the app does (can be brief for TestFlight)

#### B5.3 — Upload screenshots (from B2) ← **you are here**

1. On the same version page, find **Screenshots**.
2. Select **iPhone 6.7" Display** → drag in your PNGs from B2.
3. Add **6.5"** size too if Connect requires it.
4. **Save**.

#### B5.4 — Keywords and promotional text (optional for now)

- **Keywords:** optional; comma-separated search terms
- **Promotional text:** optional; can skip for TestFlight

---

### B6. Archive the app in Xcode

1. Select destination: **Any iOS Device (arm64)** (not Simulator).
2. **Product → Archive**.
3. Organizer → **Distribute App** → **App Store Connect** → **Upload**.

---

### B7. Wait for processing

App Store Connect → **TestFlight** → wait until build leaves **Processing**. Answer **Missing Compliance** (encryption: usually **No**).

---

### B8. Internal testing

TestFlight → **Internal Testing** → add yourself → install **TestFlight** app on iPhone → install **If I Must** → test add/search/backup/menu.

---

### B9. External testing (optional)

External group → invite by email → may need brief Beta App Review.

---

### B10. Public App Store (later)

Complete listing → select build → **Submit for Review**. **Requires Part C (subscription)** if the app will charge $4.99/year at launch.

---

## Part C — Subscription ($4.99/year + 2-week free trial)

**When:** After TestFlight feedback (B7–B9). **Before:** Public App Store submit (B10). **Not needed** for friends on TestFlight (they are not charged).

Do in order:

### C1. Agreements, Tax, and Banking

1. App Store Connect → **Business** (or **Agreements, Tax, and Banking**).
2. Sign **Paid Applications** agreement.
3. Complete tax and banking info so Apple can pay you.

*Plain English:* Apple requires this before any paid subscription can go live.

---

### C2. Create subscription in App Store Connect

1. Your app → **Subscriptions** (or **In-App Purchases** → Subscription group).
2. Create a **subscription group** (e.g. “If I Must Premium”).
3. Add product: **$4.99 / year** (auto-renewable).
4. Add **introductory offer**: **2 weeks free**.
5. Note the **Product ID** (you’ll use it in Xcode).

*Plain English:* Defines price and trial on Apple’s side. No app code yet.

---

### C3. StoreKit code in Xcode

**Status: in progress** — code added (`SubscriptionManager`, `PaywallView`, `RootView`). Product ID: `App.IfIMust.premium.yearly`.

**You must do in Xcode (one time):**

1. Open **If I Must** target → **Signing & Capabilities** → **+ Capability** → **In-App Purchase**.
2. Build and run on a **physical iPhone** (sandbox testing in C4).

**Reminder — subscription review screenshot (before C5 / public submit):**

- [ ] Replace the **placeholder** review screenshot on **`App.IfIMust.premium.yearly`** with a screenshot of the **paywall** screen (App Store Connect → Subscriptions → product → **App Review Information**). Apple uses this for review only; it is not shown on the public App Store listing. Do this **after C3 paywall is on device**, and **before** you submit the app version for App Review (C5).

1. Add StoreKit 2: load products, purchase, **Restore Purchases**, check active subscription. **Done in code.**
2. Wire Product ID from C2. **Done:** `App.IfIMust.premium.yearly`
3. Gate app until user is subscribed or in trial. **Done:** hard paywall via `RootView` (no Home without subscription/trial).

*Plain English:* The app learns whether someone has paid or is in the free trial.

---

### C4. Test subscription (Sandbox)

1. App Store Connect → **Users and Access** → **Sandbox** → create test Apple ID.
2. On iPhone: **Settings → App Store → Sandbox Account**.
3. Run app → test subscribe, trial, restore. No real money is charged.

---

### C5. App Store listing + submit with subscription

1. Confirm subscription appears on the app’s App Store page.
2. Paywall shows Privacy + Terms links (you have privacy.html and terms.html).
3. Submit for review (B10) with subscription enabled.

---

## Suggested order (short)

| Step | What |
|------|------|
| ~~1–3~~ | ~~CloudKit: portal, Xcode, test on iPhone~~ **Done** |
| ~~4~~ | ~~**B1 App icon** (1024×1024)~~ **Done for now** |
| ~~5~~ | ~~**B2 iPhone screenshots**~~ **Done** |
| ~~6~~ | ~~**B3** Bump build number~~ **Done** — **1.0 (1)** |
| ~~7a~~ | ~~**B4.1** App Information~~ **Done** |
| ~~7b~~ | ~~**B4.2** Privacy Policy URL~~ **Done** |
| ~~7c~~ | ~~**B4.3** App Privacy → Publish~~ **Done** |
| ~~7d~~ | ~~**B4.4** Pricing (Free download)~~ **Done** — **B4 complete** |
| ~~8a~~ | ~~**B5.1** Support URL~~ **Done** |
| ~~8b~~ | ~~**B5.2** Subtitle + description~~ **Done** |
| **8c** | **B5.3** Upload screenshots ← **you are here** |
| 8d | **B5.4** Keywords (optional) |
| **9** | **B6** Archive → Upload |
| **10** | **B7–B8** TestFlight processing → test yourself |
| **11** | **B9** External TestFlight → invite friends |
| **12** | **C1–C4** Subscription: Connect + StoreKit + Sandbox test |
| **13** | **B10** Public App Store submit (with $4.99/year live) |
| **14** | **Other languages** (when you choose) |
| **15** | **Revisit app icon (B1)** (optional, before public release) |

---

## What you can skip for now

- **Part C (StoreKit / $4.99 subscription)** — not needed until after TestFlight; friends are not charged during beta.
- **Add other languages** — not required for TestFlight.
- **Duplicate node bug** — revisit later.
- **Simulator CloudKit** — your app intentionally uses local data there.

---

## When you want help in Cursor

Say one of these:

- *“Do the CloudKit code re-enable steps in Xcode for me.”*
- *“Walk me through Archive upload live.”*
- *“Help me fill App Privacy answers for If I Must.”*
- *“Walk me through subscription setup (Part C).”*

*Checklist created for Randy Bevis — If I Must, bundle `App.If-I-Must`.*
