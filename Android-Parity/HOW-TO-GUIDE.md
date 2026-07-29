# How-To Guide — full English copy (iPhone source of truth)

**In-app location:** Menu → **How-To Guide** (not inside App Settings; Settings is a separate screen).  
**Source:** `MainMenuViews.swift` → `QuickGuideView`  
**YouTube:** top link opens `https://www.youtube.com/@IfIMustLLC`  
**Label:** Watch Tutorial Video (red, play.circle.fill icon, centered)

Body lines in the app are prefixed with an em dash (`— `). Example lines under “Examples:” use a bullet (`• `).

---

## Top link

**Watch Tutorial Video**  
URL: https://www.youtube.com/@IfIMustLLC

---

## How to Add Contact on the Home Page

— Each word becomes a searchable node or attribute.

— Type in the Add Contact field, then press Return (Done) on the keyboard or tap Add bubble above the keyboard.

— Format: the first word is the Master (contact base). All words after that are attributes. On add, the first two words become primary attributes. (e.g. entering “tom tall mechanic tupelo piano” creates Contact “tom tall mechanic” with Attributes “tupelo” and “piano” searchable as linked attributes).

— Use commas to separate groups — the first word in each group is the Master for that group.

— Use . to concatenate words into one node (e.g. New.York → “New York”, one space between).

— A single-word group creates a container Master with no Primaries yet (e.g. “BBQ.7/4/26” for an event).

— All contact and attribute text is stored in lowercase.

— Period and comma: your language's usual marks work too (e.g. 。，、।) — the same as . and , for joining words and separating groups.

— Tap Show Hints under the field for more help. Turn hints off in Menu → App Settings → App Hints, or turn on Minimalist to hide hints and section labels.

**Examples:**

• “Bill short old car.dealer” creates Contact “bill short old” with additional Attribute “car dealer”.

• “James tan car.dealer, Sally tall teacher glasses, Fido old dog brown” creates three linked Contacts (“james tan car dealer”, “sally tall teacher”, “fido old dog”), each with extra Attributes as entered.

---

## How to Search on the Home Page

— Type in the Search field. Results update live in the gray box as you type — contacts and attributes.

— Search matches the start of any word in a name or attribute (e.g. “banker” or “dog” can find a multi-word attribute like “tall banker houston” or “fido little black dog”).

— Use one or more keywords separated by spaces for AND search (e.g. dog brown).

— Optional: press Return or tap Search bubble above the keyboard to save the query to Recent Searches. Menu → Recent Searches lists up to 20 past queries (no expiration).

---

## Browsing on the Home Page

— Three buttons below search: All, Recent, and Favorites.

— All lists Contacts only (Master + Primary display names) — not standalone Attributes. Attributes still appear in Search.

— Recent lists Contacts you created recently. Use the menu at the top to filter Past Day, Past Week, or Past Month.

— Favorites lists contacts and attributes you pinned with Add to Favorites on their page. Demoted contacts can stay in Favorites even when they are not in All.

— Drafts/Notes: below the three buttons, tap the row to open a scratch pad (up to 5000 characters) for email drafts or other text. Saved on this device only — not part of contacts, search, or JSON backup.

---

## Editing Info on a Master Page

— One field sits below the contact name. Under it, a short line describes the active mode. Three buttons choose the mode: Word (default, black when selected), Link, and Make.

— Word — adds keywords or attributes to the current contact. Enter text, then press Return or tap Add Info bubble above the keyboard.

— Word — Sentence radio button (right of Add Info): when selected, text is treated naturally — spaces, periods, and commas keep their normal meaning, and the whole phrase becomes one attribute.

— Link — links an existing contact to the current contact. Search works like Home page (any word; multiple keywords for AND search). Tap a contact in the results to connect.

— Make — creates a new contact AND links it to the current contact at the same time. Same rules as Home page Add Contact: first word is Master, following words primary attributes.

— Concatenate: use . between words (e.g. new.york → New York).

— There can be 0-3 primary attributes. Tap X beside the contact name to clear all Primaries. Tap the green + on a connection row (tap order = display order).

— Amend Master name: tap the pencil beside the title to edit only the Master word (e.g. “tom” → “thomas”). Press Return or tap Amend bubble above the keyboard. No need for . to concatenate here.

— The Connections/Attributes section lists linked nodes. Tap the red X on a row to remove that connection and drop it from the display name if it was a Primary.

— Add to Favorites: tap the row with the star on the right (outline = not pinned; filled = pinned). In Minimalist mode the row shows only the star. Works on Masters and attributes.

— General Info: tap the notes preview to open the full editor; tap X to close and save (max 1000 characters, not searchable)

— Tap Show Hints under the field for a short summary. App Hints Off hides hints; Minimalist On also hides hints plus section headers and helper labels under fields.

---

## Other Options on the Master Page

— Add Phone and email on the Master page, press Return or tap Save above the keyboard. Tap the pencil to edit. Use Call, Text, and Email below the fields (grey when empty; black when ready).

— Bottom button row: Phone, Demote or Promote, and Delete.

— Phone exports the contact to your phone’s Contacts app (first-letter capitalization on export). Tap the person-with-plus icon when Symbols are on.

— Demote removes the contact from All and Recent lists but keeps connections and search. Promote lists it in All again. Connections stay either way.

— Delete removes the node and its connections from the app. Password protected via your phone’s passcode or Face ID / Touch ID when enabled.

---

## App Settings & Backup

— Open App Settings from the gear icon on the Home or Master page, or from the menu.

— App Hints: turn collapsible field hints On or Off on Home and Master pages. Minimalist: hides section headers (Connections/Attributes, Contact, General Info, Drafts/Notes, Search helper, etc.), helper lines under the Master field, and Show Hints — placeholders stay visible. Symbols: show icons instead of short button labels on Home and Master rows (helpful for long translations). Theme: Light or Dark. Grip Hand: place reorder grips on the left or right (Left also mirrors Master row controls). Passcode: require device lock before removing a connection (red X). Text: adjust font size; iPad 1 and iPad 2 are intended for iPad screens.

— Backup: email or import a JSON backup of your graph.

— Delete Empty Containers (App Settings): removes attribute-only nodes with no connections left after you disconnect them. Contact Masters are kept.

— App Info (in the menu) shows version, iCloud note, and a link to www.ifimust.com.

---

## Presentation notes for Android

- Screen title: **How-To Guide**
- Section titles: semibold ~title3
- Body: slightly softer than pure black (~84% opacity on iPhone)
- Top YouTube row: red play icon + **Watch Tutorial Video**; opens channel URL in browser / YouTube app
- Note: Home button UI label is **Contacts**; How-To text still says “All” in places — keep How-To wording as shipped on iPhone unless you intentionally update both platforms
