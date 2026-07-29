# DO NOT BUILD — deprecated / never-shipping UI

Copy this list into every Flutter agent chat. Building any of this is how Android drifted from the shipping iPhone app.

## Forbidden (removed or never shipped)

| Idea | Why forbidden | Build this instead |
|------|---------------|-------------------|
| Trailing **profile / avatar** icon | Removed | **Gear** → App Settings |
| Connections as wrapping **tag chips / bubbles** | Removed | Ordered **list rows** with X / name / + / grip |
| Modifier prefixes **`d*`**, **`p*`**, **`u*`** | Never shipping | Red X, green +, Word/Link/Make UI |
| Standalone **Quick Add** screen as primary add UX | Orphaned | Home **Add Contact** field |
| Phone/email as blue **`tel:` / `mailto:`** links | Removed | **Call / Text / Email** buttons (grey→black) |
| **Copy** icons beside phone/email | Tried then removed | Long-press copy only if needed |
| Connections/Attributes **above** Word/Link/Make | Wrong layout | List **below** Favorites / mode row |
| Bare red **X** (no circle) for disconnect | Wrong chrome | **Filled** red circle with X |
| Missing green **+** or reorder **grip** | Incomplete | Full connection row chrome |
| Home button labeled only **“All”** | Outdated wording | Button **Contacts**; list title **All Contacts** |
| Early `DESIGN_REFERENCE.md` / old layout PNGs as primary spec | Historical | `Android-Parity/` screenshots + md |

## Allowed current product (must build)

- Hamburger + **If I Must** title (→ Home) + **gear**
- Home: Add Contact, Search + results box, Contacts/Recent/Favorites, Drafts/Notes, encouragement
- Master: Word/Link/Make, Sentence radio (Word), Favorites star, Connections/Attributes rows, Contact, General Info, bottom Phone/Demote|Promote/Delete
- Settings: Hints, Symbols, Minimalist, Passcode, Theme, Grip Hand, Font Size, Delete Empty Containers, Backup
- Menu: Home, How-To (+ YouTube), App Settings, Why I Built, Recent Searches, App Info, Contact
- Full-row reorder drag; font scale on text **and** icons

Details: sibling `Android-Parity/` (especially `00`, `05`, `08`, `09`, How-To, Why I Built).
