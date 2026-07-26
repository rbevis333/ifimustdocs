# If I Must — Design Reference

> Visual target from **memory_app_layout.png** (this Design folder). Use with `../If I Must Specs/NAME_MEMORY_APP_SPEC.md` and `../If I Must Specs/DEVELOPMENT_PLAN.md`.

**App name:** **If I Must**

---

## Layout (from reference image)

### Header bar
- **Left:** Hamburger menu (three horizontal lines) — opens navigation drawer
- **Center:** App title **"If I Must"** in bold
- **Right:** Circular profile/avatar icon — account or settings

### Main content (Node Detail–style screen)
1. **Primary entry** — One large rounded bubble showing the current node (e.g. **"Cody bald"**). Main focus of the screen.
2. **Tags/chips** — Smaller rounded chips for connections/keywords, wrapping in rows (e.g. bald, welder, garden, cat, Kayla, peppers, George). Tappable to navigate to that node.
3. **Add Info field** — Single-line rounded text field for adding connections and `...` primary (light grey placeholder).
4. **Home hint** — Below Add Contact: concatenation with **`.`** (e.g. `new.york` → New York). No `d*`, `p*`, or `u*` modifiers.
5. **Additional Info** — Section heading **"Additional Info"** (spec uses “General Info”; same concept).
6. **Notes area** — Large rounded multi-line text area for free-form notes (e.g. “Kayla is fiance, together 5 years…”). Single-line preview when collapsed; tap to expand (per spec §6).

### Footer
- Short message, e.g. **"You're doing great. Have an amazing day."**

---

## Styling
- **Background:** White (or user-chosen background color per §13).
- **Text:** Black (or user-chosen font color).
- **Placeholders / hints:** Light grey.
- **Components:** Rounded rectangles (bubbles, chips, input, notes area).
- **Spacing:** Vertical stack with clear spacing between sections.

---

## Reference image
- **File:** `memory_app_layout.png` (this Design folder)
- Use for: header layout, chip arrangement, Add/Edit placement, Additional Info section, footer, and overall tone.
