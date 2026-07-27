# 05 — Master page (node detail)

**Screenshots:** `10-master-top.png`, `11-master-bottom.png`, `12-master-symbols-minimalist.png`

This is where Android parity is usually worst. Match **order**, then **row chrome**, then modes.

---

## CRITICAL: vertical order

```
Nav: [◀] [☰]  If I Must  [⚙]

1. Title row: [✏?]  display name          [ⓧ clear primaries?]
2. Add Info field                    (○ Sentence)   ← Sentence only in Word
3. Helper line (mode-specific)                      ← hide Minimalist
4. [ Word | Link | Make ]                           ← Word selected = black
5. Link results box                                 ← Link mode + query only
6. Favorites row: "Add to Favorites ›" …… ★        ← Minimalist: ★ only
7. Show Hints                                       ← Hints On & not Minimalist
8. Connections/Attributes header                    ← hide Minimalist
9. Connection rows (see § Connection rows)          ← BELOW Word/Link/Make
10. Contact header                                  ← hide Minimalist
11. Phone field
12. Email field
13. [ Call | Text | Email ]                         ← grey until usable
14. General Info header                             ← hide Minimalist
15. Notes preview row
16. [ Phone | Demote/Promote | Delete ]
17. Date Created / Last Edited
18. Encouragement line
```

**Wrong:** putting Connections/Attributes **above** Add Info / Word-Link-Make.  
**Right:** list is **under** Favorites (and hints), as in `10-master-top.png`.

---

## Title row

| Element | When | Look / action |
|---------|------|----------------|
| Pencil | Contact masters only | Grey pencil left of name — amend name |
| Display name | Always | Large bold (e.g. `lisa nurse blonde`) |
| Amend field | While editing | Text field replacing name; confirm Amend |
| Clear primaries | If any primary attrs | Grey `xmark.circle.fill` trailing — clears all primaries on this master |

In screenshots, `nurse` and `blonde` are primaries (name includes them) → those rows have **no** green +.

---

## Add Info + Sentence

| Mode | Field placeholder | Helper (not Minimalist) |
|------|-------------------|-------------------------|
| **Word** (default) | `Add Info` | `Add Keywords or Attributes` |
| **Link** | `Search Existing Contacts` | `Link Existing Contact` |
| **Make** | `Add Contact` | `Create New Contact` |

**Sentence** radio (Word only), to the **right** of the field:

- Default **Off**: empty circle outline (secondary)
- On: filled large-circle radio
- Hidden in Link/Make; reset Off when leaving Word
- On = treat typed text as one literal sentence (not keyword-split the same way)

---

## Word / Link / Make row

| | Selected | Unselected |
|--|----------|------------|
| Fill | Black | Light grey |
| Content | White | Black |

**Symbols On** (`12-master-symbols-minimalist.png`):

| Mode | Icon |
|------|------|
| Word | Plus `+` |
| Link | Link |
| Make | Hammer |

### Link results

- Only in Link mode with a non-empty query
- Grey rounded box **directly under** the three buttons, **above** Favorites
- Tap a hit → create link, append to connection order, clear field
- Empty: “No matching contacts to link.”

### Word / Make submit

- Word: parse keywords/attributes onto this master (Sentence changes parsing)
- Make: create new contact-style node and link

---

## Favorites row

| Minimalist Off | Minimalist On |
|----------------|---------------|
| `Add to Favorites` + chevron left; star right | Star only (trailing) |

- Empty star / filled star toggles favorite
- Filled = black star (see screenshots)

---

## Connection rows (must match exactly)

**Header:** `Connections/Attributes` (hidden Minimalist)  
**Empty:** `No connections`

### Grip Hand = Right (default) — LTR

```
[ 🔴 xmark.circle.fill ]  [ name (blue link) ]  [ 🟢 plus.circle.fill? ]  [ ≡ grip ]
```

### Grip Hand = Left — LTR

```
[ ≡ grip ]  [ 🟢 plus.circle.fill? ]  [ name (blue link) ]  [ 🔴 xmark.circle.fill ]
```

| Control | Symbol | Style | Action |
|---------|--------|-------|--------|
| Disconnect | **Filled** red circle with X | Not a bare X | Remove connection (may require auth) |
| Name | Display name | **Blue**, leading expand | Navigate to that node’s Master |
| Add primary | **Filled** green circle with + · larger (~title3) | Hidden if already primary | Mark as primary attribute |
| Grip | Three horizontal lines | Grey, large hit area | Drag reorder; persist order |

**Primary already set:** omit green + (see `nurse`, `blonde` in screenshots).  
**Not primary:** show green + (see `jim tall doctor`, `rick old smells`).

Hairline **Divider** under each row.

### Android bugs this section kills

- Bare red X without circle  
- Missing green +  
- Missing grip  
- Attributes block above Word/Link/Make  
- Non-blue names that aren’t tappable  
- No reorder persistence  

---

## Contact section

| Piece | Detail |
|-------|--------|
| Header | `Contact` (hide Minimalist) |
| Phone | Empty placeholder `Phone`; saved value + pencil to edit; **plain text** (not blue link) |
| Email | Same with `Email` |
| Call / Text / Email | Three equal buttons under fields |

**Button states:**

- No usable phone → Call & Text **light grey** (inactive)
- No usable email → Email button grey
- Dialable phone → Call & Text **black**
- Email with `@` → Email button **black**

**Symbols On:** phone / message / envelope icons (`12-master-symbols-minimalist.png`).

---

## General Info

- Header `General Info` (hide Minimalist)
- Preview: `Tap to add notes...` + `>`
- Tap → notes editor; save on dismiss

---

## Bottom action row

| Button | Symbols Off | Symbols On | Style |
|--------|-------------|------------|-------|
| **Phone** | Text “Phone” | Person + badge plus (add to device contacts) | Black |
| **Demote** or **Promote** | Demote if contact master; Promote if attribute-only | Down / up arrow | Black |
| **Delete** | “Delete” | X icon | **Red** fill, **black** label |

Confirm sheets for Demote / Promote / Delete. Delete may use device authentication. Blocked if this node is someone else’s primary attribute.

---

## Dates + encouragement

Centered under buttons:

```
Date Created: MMM d, yyyy
Last Edited: MMM d, yyyy
You're doing great. Have an amazing day.
```

---

## Minimalist + Symbols (Master) quick view

See `12-master-symbols-minimalist.png`:

- No helper under Add Info  
- No “Add to Favorites” text (star remains)  
- No section headers  
- No Show Hints  
- Word/Link/Make + Call/Text/Email + bottom row use **icons**  
- Connection rows **unchanged** (still red X circle, green +, grip, blue names)

---

## What is NOT on Master

- No Home Add/Search boxes  
- No Contacts/Recent/Favorites nav row  
- No modifier chips / syntax  
- No Sentence control outside Word  
- No green + on rows that are already primary  
