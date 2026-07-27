# 01 — Visual system (match these tokens)

Reference screenshots: all of `screenshots/01`–`12`.

## Palette (Light theme)

| Role | Appearance |
|------|------------|
| Page background | White (`systemBackground`) |
| Sheet / Settings background | Light grey grouped background |
| Primary text | Black |
| Secondary / placeholders | Medium–light grey |
| Section headers (Settings) | Small grey caps (iOS grouped style) |
| Field borders | Light grey stroke, white fill |
| Results / notes preview fill | Light grey (`systemGray5` / `systemGray6`) |
| Selected primary button | **Solid black** fill, **white** content |
| Unselected secondary button | **Light grey** fill (`systemGray5`), **black/primary** content |
| Inactive Call/Text/Email | Same light grey as unselected |
| Active Call/Text/Email | Solid black, white content |
| Destructive button | **Solid red** fill, **black** label (Delete, Delete Empty Containers) |
| Connection name | **Blue** (NavigationLink / tappable) |
| Disconnect | **Red** filled circle with white X |
| Add primary | **Green** filled circle with white + |
| Clear primaries (title) | Grey filled circle with X (secondary) |
| Favorites star filled | Black/primary filled star |
| Menu “Home” row | Blue text, **no** chevron |

## Shape & size

| Element | Spec |
|---------|------|
| Text fields | Rounded rectangle ~8–10pt radius, light border, inner padding ~10 |
| Search results box | Larger grey rounded rect ~12pt radius |
| Three-button rows | Equal width, ~6pt gap, ~8pt corner radius, vertical pad ~10 |
| Nav menu/gear/back | **Circle** buttons, white fill, soft shadow, black icon |
| Drafts/Notes & General Info preview | Grey fill, rounded (~8), trailing chevron |
| Connection row icons | Filled circles (X and +), not bare glyphs |
| Reorder grip | Three horizontal lines, grey/secondary, large hit target (~30×44) |

## Typography

- System sans (SF on iPhone → use a clean Material equivalent, not decorative display fonts)
- App title: bold, ~title3
- Master display name: bold, ~title2
- Section labels on Master/Home: subheadline semibold (e.g. `Connections/Attributes`, `Contact`, `Drafts/Notes`)
- Helpers under fields: subheadline, primary (not tiny caption grey) when shown
- Encouragement footer: bold, single line, centered, auto-shrink to fit
- Dates under Master: caption, centered

## Global navigation chrome

```
[ ◯ back? ] [ ◯ ☰ ]     If I Must     [ ◯ ⚙ ]
```

- **If I Must** tap → clear navigation stack to Home
- **☰** → Menu sheet (`04-menu.png`)
- **⚙** → App Settings sheet (`05`–`06`)
- **Back** appears on pushed screens (Master, lists); circular, same style as menu/gear

## Three-button row pattern (reuse everywhere)

Used for:

- Home: Contacts / Recent / Favorites
- Master: Word / Link / Make
- Master: Call / Text / Email
- Master bottom: Phone / Demote|Promote / Delete (Delete is red variant)

Rules:

1. Three equal columns
2. Selected / active = black + white
3. Idle = light grey + dark content
4. Symbols mode replaces text with icons (see per-screen docs)
5. Delete (and Delete Empty Containers) = red background + black text

## Do not

- Purple gradient themes, cream “AI default” palettes, heavy multi-shadow cards
- Pill clusters / stat strips on Home or Master
- Profile avatar in the trailing chrome
- Flat bare X for disconnect (must be filled circle)
- Cards around the hero content on Home (fields sit directly on white)
