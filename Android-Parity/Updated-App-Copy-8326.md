# Updated App Copy 8326

**Date:** August 3, 2026 (`8326` = 8/3/26)  
**Purpose:** Canonical How-To Guide + Home/Master Hints for **iPhone and Android** parity.  
**Audience:** Pass this file to the iPhone app agent so that app matches Android styling and wording exactly.  
**Android source of truth:** `android/app/src/main/java/com/ifimust/app/ui/MenuCopy.kt`, `HintCopy.kt`

---

## Style rules (How-To Guide)

Apply these on **both** platforms.

### Section titles (main headers)

Every section title below must be:

| Style | Value |
|-------|--------|
| Weight | **Bold** |
| Alignment | **Centered** |
| Decoration | **Underlined** |
| Examples | `Add a Contact from the Home Page`, `Search from the Home Page`, … |

Do **not** center/underline body text—only these section titles.

### Inline bold in body text

Bold **only** the following phrases/labels (listed by section). Everything else is regular weight.

| Section | Bold exactly |
|---------|----------------|
| **Add a Contact from the Home Page** | `Example:` (the standalone label line before the sample input). Do **not** bold the later phrase `Example: BBQ.7/4/26`. |
| **Browse from the Home Page** | `All`, `Recent`, `Favorites`, `Drafts/Notes` (each as its own label line) |
| **Edit a Master** | `Word`, `Link`, `Make` (each as its own label line); also bold **`Sentence Mode`** inside the Word paragraph |
| **Phone, Email, Demote, and Delete** | `Phone`, `Demote/Promote`, `Delete` (each as its own label line under the bottom-row paragraph) |
| **App Settings and Backup** | `App Hints`, `Minimalist`, `Symbols`, `Theme`, `Grip Hand`, `Passcode`, `Text`, `Backup`, `Delete Empty Containers`, `App Info` (each as its own label line) |

### Hints UI (Home & Master)

- Shown under **Show Hints** as a bullet list (`•` + text).
- Each hint is one bullet. No special bold/underline required in hints unless you later choose to bold the short title before the em dash (optional; Android currently uses plain text for the whole bullet).

### Tutorial row (How-To screen chrome)

- Label: `Watch Tutorial Video`
- URL: `https://www.youtube.com/@IfIMustLLC`
- (Android shows a red play-style link above the guide; match iPhone as already implemented.)

---

## HOW-TO GUIDE

### Add a Contact from the Home Page

*(title: bold + centered + underlined)*

Type in the Add Contact field, then press Return/Done or tap Add above the keyboard.

The first word becomes the Master (the contact's base name). Each word after it becomes a searchable attribute. The first two attributes also appear in the contact's display name.

**Example:**

tom tall mechanic tupelo piano

Creates the contact "tom tall mechanic." "tupelo" and "piano" remain linked, searchable attributes.

To keep multiple words together as one attribute, join them with a period:  
new.york → new york

To create and link multiple contacts at once, separate them with commas. The first word in each group becomes that contact's Master:  
james tan car.dealer, sally tall teacher glasses, fido old dog brown

This creates three linked contacts:

- james tan car dealer
- sally tall teacher
- fido old dog

Any remaining words become linked, searchable attributes.

A group containing only one joined entry creates a container with no Primary attributes. Example: BBQ.7/4/26

Contacts and attributes are stored in lowercase.

Your language's equivalent punctuation marks also work. For example, 。，、 and । work like periods and commas for joining words or separating groups.

Tap Show Hints below the field for a quick reminder. To hide hints, open Menu → App Settings and turn off App Hints. Minimalist also hides hints and section labels.

---

### Search from the Home Page

*(title: bold + centered + underlined)*

Type in the Search field. Matching contacts and attributes appear in the gray box as you type.

Search matches the beginning of any word in a name or attribute. For example, "banker" can find "tall banker houston," and "dog" can find "fido little black dog."

Use multiple words to narrow the results. All words must match:  
dog brown

To save a search, press Return or tap Search above the keyboard. Menu → Recent Searches stores your 20 most recent saved searches with no expiration.

---

### Browse from the Home Page

*(title: bold + centered + underlined)*

Use the three buttons below Search:

**All**  
Shows Contacts by their Master and Primary display names. Standalone attributes do not appear here, but remain searchable.

**Recent**  
Shows recently created Contacts. Use the menu at the top to choose Past Day, Past Week, or Past Month.

**Favorites**  
Shows Contacts and attributes you added to Favorites. A demoted Contact can remain here even when it no longer appears under All.

**Drafts/Notes**  
Tap the row below these buttons to open a scratch pad for drafts or other text. It holds up to 5,000 characters and stays on this device. It is not searchable and is not included in JSON backups.

---

### Edit a Master

*(title: bold + centered + underlined)*

The field below the contact name has three modes: Word, Link, and Make. A short line below the field explains the selected mode.

**Word**  
Adds keywords or attributes to the current contact. Enter the text, then press Return or tap Add Info above the keyboard.

To keep multiple words together as one attribute, join them with a period:  
new.york → new york

Turn on **Sentence Mode** by clicking the radio button to save the entire entry as one attribute. Spaces, periods, and commas then keep their usual meaning.

**Link**  
Finds and links an existing contact. Search by any word or use multiple words to narrow the results, then tap a contact to link it.

**Make**  
Creates a new contact and links it to the current contact. The first word becomes the new Master and the words after it become attributes, just like Add Contact on the Home Page.

---

### Change a Contact's Name and Primary Attributes

*(title: bold + centered + underlined)*

A Contact can have up to three Primary attributes in its display name.

- Tap the X beside the contact name to clear all Primary attributes.
- Tap the green + on a connection row to add it as a Primary. The order you tap determines the display order.
- Tap the pencil beside the title to change only the Master name—for example, "tom" to "thomas." Press Return or tap Amend above the keyboard. You do not need periods when editing the Master name here.

---

### Connections, Favorites, and General Info

*(title: bold + centered + underlined)*

Connections/Attributes lists everything linked to the current Master.

- Tap the red X on a row to remove the connection. If it was a Primary, it is also removed from the display name.
- Tap the star row to add or remove the current Master or attribute from Favorites. An outlined star is not a Favorite; a filled star is. Minimalist shows only the star.
- Tap the General Info preview to open the full editor. Tap X to close and save. General Info holds up to 1,000 characters and is not searchable.

Tap Show Hints for a quick summary. Turning off App Hints hides these hints. Minimalist also hides section headers and helper labels.

---

### Phone, Email, Demote, and Delete

*(title: bold + centered + underlined)*

Add a phone number or email address on the Master page, then press Return or tap Save above the keyboard. Tap the pencil to edit it.

Call, Text, and Email appear below the fields. A button is gray when its information is missing and black when it is ready to use.

The bottom row contains Phone, Demote/Promote, and Delete:

**Phone**  
Exports the contact to your phone's Contacts app and capitalizes the first letter. When Symbols is on, this appears as the person-with-plus icon.

**Demote/Promote**  
Demote removes the contact from All and Recent without removing its connections or search results. Promote adds it to All again.

**Delete**  
Removes the node and its connections from If I Must. When Passcode is enabled, deletion requires your device passcode, Face ID, or Touch ID.

---

### App Settings and Backup

*(title: bold + centered + underlined)*

Open App Settings from the gear on the Home or Master page, or from the menu.

**App Hints**  
Shows or hides the collapsible hints on Home and Master pages.

**Minimalist**  
Hides section headers, helper lines, and Show Hints. Field placeholders remain visible.

**Symbols**  
Uses icons instead of short button labels on Home and Master pages. This can help when translated labels are long.

**Theme**  
Choose Light or Dark.

**Grip Hand**  
Places reorder grips on the left or right. Choosing Left also mirrors the Master row controls.

**Passcode**  
Requires the device lock before a connection can be removed with the red X.

**Text**  
Adjusts the font size. iPad 1 and iPad 2 are intended for iPad screens.

**Backup**  
Email a JSON backup of your graph or import one.

**Delete Empty Containers**  
Removes attribute-only nodes that have no connections. Contact Masters are kept.

**App Info**  
Shows the app version, iCloud information, and a link to www.ifimust.com.

---

## HINTS — HOME PAGE

Display as bullets under Show Hints:

1. Add a Contact — The first word becomes the Master. Each word after it becomes an attribute. The first two attributes appear in the contact's display name. Example: john tall banker old → john tall banker. "old" remains a linked, searchable attribute.

2. Keep Words Together — Use a period to keep multiple words in one attribute: new.york → new york

3. Link Multiple Contacts — Use commas to create and link multiple contacts: jack dad, jill mom

---

## HINTS — MASTER PAGE

Display as bullets under Show Hints:

1. Change the Master Name — Tap the pencil to change only the Master name—for example, "tom" to "thomas." Primary attributes stay the same.

2. Change Primary Attributes — Tap the X beside the contact name to clear all Primaries. Tap the green + on a connection row to add it as a Primary. Tap order sets the display order. You can add up to three.

3. Choose a Field Mode — Word adds attributes to this contact. Link connects an existing contact. Make creates a new contact and links it.

4. Keep Words Together — Use a period to keep multiple words in one attribute: new.york → new york

5. Save a Sentence — Click the radio button to turn on Sentence mode to save the entire entry as one attribute. Spaces, periods, and commas keep their usual meaning.

6. Bottom Buttons — Phone exports the contact to your phone's Contacts app. Demote/Promote changes whether the contact appears under All and Recent. Delete removes the contact from If I Must.

7. Call, Text, or Email — These buttons are gray when their information is missing and black when ready.

---

## Quick checklist for iPhone agent

- [ ] Replace How-To Guide body with the sections above (exact wording).
- [ ] Section titles: **bold + centered + underlined**.
- [ ] Apply **bold** only to the inline labels listed in the Style rules table.
- [ ] Bold **Sentence Mode** in the Word paragraph (Edit a Master).
- [ ] Replace Home Show Hints with the 3 bullets above.
- [ ] Replace Master Show Hints with the 7 bullets above.
- [ ] Confirm no leftover old em-dash How-To / hint strings remain in Localizable.strings or Swift views.
