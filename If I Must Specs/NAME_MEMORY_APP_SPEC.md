# Name Memory App — Product Specification

> A flexible, graph-based app for remembering people and connections. Every word is a node; connections form a searchable memory graph.

**For Cursor on Mac:** Use this spec with **DEVELOPMENT_PLAN.md** in the same project. Reference this file (e.g. *"per NAME_MEMORY_APP_SPEC.md §3"*) when asking Cursor to implement features.

---

## Core Concept

People, places, dogs, attributes—everything is an equal node. Search by any term to find connected memories. No rigid hierarchy; the structure emerges from what you add and how you connect it.

---

## 1. Data Model

### Node
- **label** — The text (e.g., "Cody", "bald", "Texas", "George old black dog")
- **baseLabel** — For masters: the base name before numbering ("Cody")
- **numericSuffix** — 1, 2, 3 when duplicate masters exist; null otherwise
- **primaryAttributeId** — Optional; links to node used as primary attribute for display
- **isUnique** — Boolean; canonical nodes (e.g., Texas) are not duplicated
- **generalInfo** — Optional free-text notes; max 500 characters; **not searchable**
- **dateCreated**

### Connection
- **nodeA** — Reference to Node
- **nodeB** — Reference to Node
- **dateCreated**

### Display Rules
- If `primaryAttribute` set → Display as `{baseLabel} {primaryAttribute.label}` (e.g., "Cody bald")
- Else if numbered → Display as `{baseLabel} {numericSuffix}` (e.g., "Cody 1")
- Else → Display as `{label}` (e.g., "Texas")

---

## 2. Home Screen

Two entry points only:

| Element | Purpose |
|---------|---------|
| **Search bar** | Type keywords (comma-separated for multi-keyword AND search) |
| **Add button** | Open Quick Add to create new memories |

### Menu (dropdown / slide-out)
| Item | Purpose |
|------|---------|
| Navigation | Home, Add, recent searches |
| App Info | Version, credits, support |
| Account Info | Settings, backup, sync (future) |
| **Quick Guide** | Full explanation of input format and Add Info behavior |

---

## 3. Input Format (Add Screen)

### Rules
| Character | Meaning |
|-----------|---------|
| **Space** | Separates words — each word becomes a node |
| **Comma** | Starts a new group — the next word is that group's master |
| **+** | Concatenate — joins adjacent words into one node with exactly one space between them |

### Concatenation Modifier (+)
Use `+` when a node needs two or more words (e.g., "Texas State", "San Antonio").

**Equivalent inputs — all produce "Texas State" (one space):**
| Input | Result |
|-------|--------|
| `Texas + State` | Texas State |
| `Texas+State` | Texas State |
| `Texas +State` | Texas State |
| `Texas+ State` | Texas State |

**Rule:** Regardless of spaces around `+`, the output node has exactly one space between concatenated parts.

### Example (with concatenation)
**Input:** `Cody bald Texas + State garden, George old black dog, Kayla fiance`

| Group | Master | Other Nodes |
|-------|--------|-------------|
| 1 | Cody | bald, **Texas State**, garden |
| 2 | George | old, black, dog |
| 3 | Kayla | fiance |

*("Texas + State" concatenates into the single node "Texas State".)*

### Connections
- **Within each group:** Master connects to every other word (or concatenated node) in that group.
- **Between masters:** All masters in the same add are connected to each other.

**Result:** Cody ↔ bald, Texas State, garden, George, Kayla | George ↔ old, black, dog, Cody, Kayla | Kayla ↔ fiance, Cody, George

---

## 4. Duplicate Masters

When the same master name appears multiple times:
- First: `Cody`
- Second: `Cody 2`
- Third: `Cody 3`

### Primary Attribute
Assigned via Edit to differentiate duplicates and improve search display:
- Without: "Cody 1", "Cody 2", "Cody 3"
- With: "Cody bald", "Cody tall", "Cody old"

Primary can be an existing connected node or a new keyword added in Edit.

---

## 5. Canonical nodes (`isUnique`)

The data model may include an **isUnique** flag for nodes that should not be duplicated (e.g. a place name). The app does **not** expose text commands such as `d*`, `p*`, or `u*` to set this. Duplicate people are distinguished by **composite master labels** and **primary attributes** (see §4 and §6).

---

## 6. Edit Screen (Node Detail — Add Info)

Same word/concatenation logic as Add (spaces separate words; `.` or `+` concatenate with one space), but scoped to the **current node** (no comma/group parsing).

**Example:** On Kayla node → Add Info → type `doctor blonde 37`  
→ Adds doctor, blonde, 37 as connections to Kayla.

**Example with concatenation:** On Kayla node → Add Info → type `Texas.State doctor`  
→ Adds "Texas State" and "doctor" as connections to Kayla.

### Add Info behavior (not `d*` / `p*` / `u*`)

| Input / action | Meaning | Example |
|----------------|---------|---------|
| Words (spaces) | Add nodes and connect to current node | `truck cat` |
| `.` or `+` | Concatenate adjacent words (one space) | `Texas.State` → Texas State |
| `...` + attribute | Set **primary attribute** for display | `... smith` on george → display **george smith** |
| `...` alone | Clear primary attribute | `...` |
| Red **X** on a connection row | Remove that connection only | Tap X next to welder |

**Removed from spec (no longer used):** `d*` (delete connection), `p*` (set primary), `u*` (mark unique). Use the table above instead.

### Node Detail Layout (top to bottom)

1. **Connections** — List with red **X** to remove; tap name to open that node  
2. **Add Info** — Single-line field (placeholder explains `...` for primary)  
3. **General Info** — Free-edit notes (multi-line field in current app; popup optional per implementation)  

### General Info (Node Detail only)

A free-edit text field at the bottom of the Node Detail page.

| Property | Value |
|----------|-------|
| **Location** | Node Detail page, at bottom (below Add/Edit) |
| **Max length** | 500 characters |
| **Searchable** | No — supplementary notes only |
| **Display** | Single-line preview (truncated to fit); tap to open full editor |

**Behavior:**
1. **Collapsed view:** Single line showing as much text as fits; ellipsis if truncated.
2. **Tap to open:** Full-screen popup (most of screen) with text editor.
3. **Popup:** X button top-right to close; saves content on close.
4. **To view full text:** Tap the General Info field again to reopen the popup.
5. **Popup scrolling:** Text area is finger-scrollable when content exceeds visible area.

**Example content:**  
"He has had his cat for seven years. He used to play baseball. He broke his arm in the 7th grade."

---

## 7. Search

### Single keyword
- Type `dog` → returns all nodes connected to "dog"

### Multi-keyword (AND logic)
- Type `dog, old` → returns nodes connected to BOTH "dog" AND "old"
- Type `dog, old, black` → returns nodes connected to ALL THREE
- Example: narrows down to find "George" (the old black dog)

---

## 8. Screens Summary

| Screen | Purpose |
|--------|---------|
| **Home** | Search bar + Add button + Menu |
| **Add** | Single text field for Quick Add (comma/space parsing) |
| **Search Results** | Nodes matching the query |
| **Node Detail** | Connections (top) → Add/Edit (middle) → General Info (bottom) |

---

## 9. Quick Guide Content (for in-app menu)

### How to Add a Memory
- Each **word** becomes a searchable node.
- Use **commas** to separate groups — the first word after each comma is the master for that group.
- Use **+** to concatenate words into one node: `Texas + State` → "Texas State" (always one space between).

**Example:** `Cody bald Texas + State garden, George old black dog, Kayla fiance`  
→ Cody (master) + bald, Texas State, garden | George (master) + old, black, dog | Kayla (master) + fiance  
→ Cody, George, Kayla also linked to each other.

### How to Search
- One or more keywords.
- Use **commas** to narrow results (AND search): `dog, old, black`.

### Concatenation (+)
- `Texas + State` or `Texas+State` → Creates node "Texas State" (one space)
- Works in both Add and Edit

### Add Info (Node Detail)
- `truck cat` → Add truck, cat as connections
- `Texas.State` or `Texas + State` → Add "Texas State" as one connection
- `... bald` → Set **bald** as primary attribute (display e.g. **Cody bald**)
- Red **X** → Remove a connection

### Primary Attribute
Used to differentiate duplicate masters. Set with **`...`** in Add Info (not `p*`).

### General Info
Free-form notes (max 500 chars) on each node. Not searchable. Tap to open full-screen editor; X to close and save. Single-line preview when collapsed.

---

## 10. Edge Cases

| Scenario | Handling |
|----------|----------|
| Single word before comma | One-node group |
| Trailing comma | Ignore empty group |
| Duplicate word in same group | Create node once, avoid duplicate connections |
| Same word as master elsewhere | Reuse node; add new connections |
| Empty group (`,,`) | Ignore |
| `... newword` when not connected | Create/find newword, connect, set as primary |
| `...` with empty remainder | Clear primary attribute |
| Remove connection | Use red **X** on that row (not `d*`) |
| `Texas  +   State` (multiple spaces) | Output: "Texas State" (one space only) |
| `+` with only one adjacent word | Treat + as invalid or ignore; single word stands alone |
| General Info over 500 chars | Truncate or block input at 500 |
| General Info empty | Show placeholder (e.g., "Tap to add notes...") |

---

## 11. Security / Input Validation

Prevent exploitation of Search, Add/Edit, and General Info fields through length limits, sanitization, and validation.

### Length Limits

| Field | Max Length |
|-------|------------|
| Search | 500 characters |
| Add / Edit (full input) | 2,000 characters |
| Node label (per word/node) | 100 characters |
| General Info | 500 characters |
| Nodes per add | 50 nodes |

### Input Sanitization (all fields)

1. **Trim** leading and trailing whitespace
2. **Remove** control characters (0x00–0x1F, 0x7F) and non-printable Unicode
3. **Normalize** Unicode (e.g., NFKC) for consistent storage and comparison
4. **Enforce** length limits before processing — block or truncate at limit

### Field-Specific Safeguards

| Field | Safeguards |
|-------|------------|
| **Search** | Length limit; strip control chars; ignore empty/whitespace-only |
| **Add / Edit** | Length limit; max nodes per add; max length per node; sanitize before parsing |
| **General Info** | 500-char limit; strip control chars; normalize Unicode |

### Implementation Notes

- Use SwiftData/Core Data with model bindings — never build query strings from raw user input
- Use native text views (`Text`, `TextEditor`) — no HTML/script interpretation
- Store and display as plain text only — never execute user input as code

---

## 12. Data Storage

- **No server / no user data on your backend.** All data stays on the user’s device or in the user’s iCloud.
- **Local:** SwiftData or Core Data stores nodes and connections in the app’s container on the device.
- **Optional iCloud:** Enable iCloud + CloudKit; use SwiftData with CloudKit or `NSPersistentCloudKitContainer` so the user’s data syncs to their private iCloud. Data is in the user’s iCloud account, not on developer servers.
- **Privacy:** Declare “data stored on device” / “data in user’s iCloud” in App Store Connect and in the app’s privacy policy.

---

## 13. Appearance Settings (Current Scope)

Appearance remains intentionally minimal in **App Settings**.

| Option | Implementation |
|--------|----------------|
| **Theme** | Light / Dark toggle |
| **Font size** | Small / Medium / Large step control |

- No advanced color customization (for example, `ColorPicker`) is planned in this scope.
- Keep defaults streamlined and consistent across screens.

---

## 14. Technical Notes (for implementation)

- **Storage:** SwiftData or Core Data (graph of nodes + connections); device + optional iCloud only (§12).
- **UI:** SwiftUI; apply the minimal appearance settings from §13.
- **Platform:** iOS 17+ (SwiftData) or iOS 16+ (Core Data)
- **Search:** Match node labels; filter by connections for multi-keyword AND logic.
- **General Info popup:** Use scrollable text view (e.g., `ScrollView` + `TextEditor`) so long content is finger-scrollable.
- **Input validation:** See Section 11 (Security / Input Validation).

---

*Spec created from planning session. Use with DEVELOPMENT_PLAN.md in Cursor on Mac + Xcode.*
