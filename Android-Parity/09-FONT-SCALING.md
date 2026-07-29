# 09 — Font size scaling (text AND chrome must scale together)

**Bug on Android:** Changing Settings → Text → Font Size only resizes text. Red X, green +, grips, radios, stars, and often buttons stay fixed — so at **X Small** icons look huge next to tiny attribute text.

**Correct iPhone behavior:** App font size sets a **global Dynamic Type / content-size category**. Almost all UI that uses semantic text styles (including SF Symbols with `.body` / `.title2` / `.title3`) **scales with that category**. Connection-row red X and green + shrink/grow with the attribute name.

---

## What to tell the Windows agent (copy/paste)

```text
git pull ifimustdocs. Read Android-Parity/09-FONT-SCALING.md.

Fix Font Size so it is a GLOBAL UI scale, not text-only.

When the user changes Settings → Text → Font Size:
1. Apply one scale factor (relative to default "Small" = 1.0) to:
   - All body/title/caption text
   - Master connection-row icons: red xmark.circle.fill, green plus.circle.fill, reorder grip
   - Title-row pencil and clear-primaries X
   - Sentence radio circles
   - Favorites star
   - Section headers, helpers, dates, encouragement (as applicable)
2. Button ROW HEIGHT / vertical padding should grow with the scale (labels stay readable).
3. Do NOT leave connection-row icons at a fixed dp while text uses sp that changes with fontSizeStep.

Use the scale table in 09-FONT-SCALING.md (Small = 1.00 baseline).
X Small ≈ 0.90, Medium ≈ 1.11, Large ≈ 1.21, iPad 1 ≈ 1.47, iPad 2 ≈ 1.74.

Implementation hint: one CompositionLocal / theme fontScale multiplier applied to TextStyle AND Icon size (and hit targets where needed).
```

---

## How iPhone does it (mechanism)

Source: `If_I_MustApp.swift` + `AppFontSize.swift`

```swift
.environment(\.sizeCategory, AppFontSize.sizeCategory(for: fontSizeStep))
```

Settings steps map to Apple `ContentSizeCategory` (not a custom per-widget font):

| Step | Settings label | ContentSizeCategory | Approx **body** pt | Scale vs **Small** |
|------|----------------|---------------------|--------------------|--------------------|
| −2 | **X Small** | `.large` | 17 | **0.90** |
| −1 | **Small** (default) | `.extraLarge` | 19 | **1.00** |
| 0 | **Medium** | `.extraExtraLarge` | 21 | **1.11** |
| 1 | **Large** | `.extraExtraExtraLarge` | 23 | **1.21** |
| 2 | **iPad 1** | `.accessibilityMedium` | 28 | **1.47** |
| 3 | **iPad 2** | `.accessibilityExtraLarge` | 33 | **1.74** |

Point sizes are approximate UIKit Dynamic Type for **body**. Ratios (last column) are what Android should mirror.

**Rule of thumb:**  
`scaledSize = baseSizeAtSmall × scale(step)`

Example: green + uses ~`.title3` (~22pt at Small) → at X Small ≈ `22 × 0.90 ≈ 20`; at iPad 2 ≈ `22 × 1.74 ≈ 38`.

---

## What MUST scale with font size (parity-critical)

These look wrong on Android today if only text changes:

| Element | iPhone sizing approach | Android should |
|---------|------------------------|----------------|
| Attribute / connection **name** | Semantic text (body) | Scale with step |
| Red disconnect **`xmark.circle.fill`** | Default / scales with Dynamic Type | **Same scale as name** (not fixed 24dp) |
| Green primary **`plus.circle.fill`** | `.font(.title3)` → scales | **Same family as title3 × scale** |
| Reorder **grip** `line.3.horizontal` | `.font(.body)` → scales | Scale; keep ~30×44 **minimum** hit target, visual glyph scales |
| Clear-primaries X (title) | `.font(.title3)` | Scale |
| Pencil (amend) | `.font(.body.weight(.medium))` | Scale |
| Sentence radio | `.font(.title2)` | Scale |
| Favorites **star** | `.font(.title3)` | Scale |
| Section headers / helpers | `.subheadline` etc. | Scale |
| Master display name | `.title2.bold()` | Scale |
| Dates / captions | `.caption` | Scale |
| Three-button **text** labels | Measured from Dynamic Type text style | Scale (+ may shrink slightly to fit width, min ~10pt) |
| Three-button **row height** | `lineHeight(textStyle)` + vertical pad 10 | Grow with font |
| Field placeholders / values | Form body style | Scale |
| Nav title “If I Must” | Fixed `.title3.bold()` on iPhone | Prefer slight scale or keep stable; less critical than connection icons |

---

## Special case: Symbols-mode button icons (iPhone quirk)

On iPhone, when **Settings → Symbols** is On, the **icons inside** Home Contacts/Recent/Favorites and Master Word/Link/Make / bottom actions use **fixed point sizes** (~16–18pt) so they stay compact in the black buttons.

Connection-row X / + / grip are **not** in that exception — they always track Dynamic Type.

**Android recommendation for parity + less weirdness:**

1. **Required:** scale connection-row and title-row chrome with the font scale table.  
2. **Recommended:** also scale Symbols-mode button icons by the same factor (or `lerp` slightly less aggressive, e.g. `0.5 + 0.5×scale`) so X Small does not leave giant glyphs in black pills. Matching iPhone’s fixed Symbols icons exactly is secondary to fixing the connection-row bug.

---

## Suggested Android implementation

### 1. Single source of truth

```kotlin
enum class AppFontStep(val scale: Float) {
  X_SMALL(0.90f),
  SMALL(1.00f),      // default
  MEDIUM(1.11f),
  LARGE(1.21f),
  IPAD_1(1.47f),
  IPAD_2(1.74f);
}
```

Expose via `CompositionLocalProvider(LocalAppFontScale provides step.scale)`.

### 2. Apply to typography AND icons

```text
textSp = baseSp * scale
iconDp = baseIconDp * scale
buttonMinHeight = baseMinHeight * scale   // or at least label line height × scale + padding
```

Do **not** only change `MaterialTheme.typography` while leaving `Icons.Default` / `Modifier.size(24.dp)` hard-coded.

### 3. Base sizes at Small (= 1.0) — targets

| Chrome | Base at Small (≈) | Notes |
|--------|-------------------|--------|
| Connection name | body ~19sp | |
| Red X glyph | ~ body / default icon ~19–22dp visual | Must track name |
| Green + glyph | ~ title3 ~22dp | Explicitly larger than X on iPhone |
| Grip glyph | body ~19sp | Frame hit ~30×44dp floor |
| Star / clear X | title3 ~22dp | |
| Sentence radio | title2 ~28dp class | |
| Three-button label | subheadline / body | Uniform per row; may shrink to fit |
| Black button vertical pad | 10dp × scale (or keep 10 and grow label height) | Row must not clip |

Exact pixels need not match iOS; **relative** scale between steps and **relative** size of X vs name vs + must stay balanced.

### 4. Quick visual test

At **X Small**: red X and green + should look **slightly smaller** than at Small, still balanced with attribute text — never “massive circles + tiny text.”  
At **iPad 2**: icons and text both large; rows taller; still one coherent row.

---

## Checklist

- [ ] Font Size changes use one global scale table (Small = 1.0)
- [ ] Connection-row red X scales
- [ ] Connection-row green + scales
- [ ] Grip glyph scales (hit target stays usable)
- [ ] Attribute text and icons stay proportional at X Small and iPad 2
- [ ] Button rows get taller / labels larger with scale
- [ ] No hard-coded 24.dp icons on Master connection rows

---

## Related

- Settings UI: `04-SETTINGS.md`
- Connection row chrome: `05-MASTER.md`
- iPhone sources: `AppFontSize.swift`, `If_I_MustApp.swift`, `NodeDetailView.swift` (`.font(.title3)` on green +)
