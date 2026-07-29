# 08 — Reorder grip (full row moves, not just the grip)

**Problem on Android today:** only the grip icon moves while dragging.  
**Correct iPhone behavior:** the **entire row** moves together — red X circle, attribute name, green + (if present), and grip — as one floating unit. Other rows stay still until you release; then the list order commits.

There is **no separate PNG asset in the repo** for this. At drag start, iOS **renders a snapshot image of the whole row** and moves that image under the finger.

---

## What to tell the Windows agent (copy/paste)

```text
Fix Master (and Favorites) reorder drag to match iPhone.

Wrong: only the grip icon translates while dragging.
Right: the FULL row floats with the finger: red filled-circle X + blue name + green filled-circle plus (if shown) + grip.

iPhone implementation (ReorderGrip.swift):
1. Drag gesture is ONLY on the grip (not on the whole row).
2. On drag start: capture a bitmap/snapshot of the full floating row (grip + row content) via ImageRenderer-equivalent.
3. While dragging:
   - Keep an invisible layout slot so the list does not collapse.
   - Hide the real row content (opacity 0).
   - Almost-hide the live grip (opacity ~0.01) so it still receives the gesture.
   - Draw the snapshot image on top, translated by dragY (vertical only).
4. Other rows do NOT animate/swap during the drag — they stay put.
5. On release: compute target index from dragY / rowHeight, move item in the list, persist order, clear snapshot.
6. Grip Hand Left/Right only changes which side the grip sits on (and mirrors X/+/name on Master); drag behavior is the same.

See Android-Parity/08-REORDER-DRAG.md and Master screenshots 10–12 for row chrome.
Do not ship “grip-only” drag.
```

---

## iPhone mechanics (shipping behavior)

Source: `ReorderGrip.swift` + `NodeDetailView` connection rows / `FavoritesView`.

### Components

| Piece | Role |
|-------|------|
| `ReorderGrip` | Three-line icon; **only** control with the vertical `DragGesture` |
| `ReorderRowContainer` | Layout slot + snapshot overlay |
| `floatingRow` | Full HStack: grip + `row(floating: true)` (X, name, +, etc.) |
| `dragSnapshot` | `CGImage` from `ImageRenderer(content: floatingRow)` at drag start |
| `ReorderableListState` | Tracks `activeToken`, `dragY`; commits index on `finishDrag` |

### While dragging (visual)

```
[ layout slot stays — real row opacity 0 ]
[ snapshot of FULL row follows finger vertically ]
[ live grip nearly invisible but still tracking finger ]
[ other rows do not move until release ]
```

Comment in code: *“Invisible slot keeps layout; grip tracks the finger. A snapshot image moves on screen.”*

### On release

1. `steps = round(dragY / rowHeight)` (Master uses ~52pt row height)
2. Clamp target index into list bounds
3. Animate list reorder once
4. Light haptic
5. `onCommit` persists order (Master: connection order; Favorites: favorite order)
6. Clear snapshot / drag state

### Floating vs interactive row content

When `floating == true`, controls are **non-interactive copies** (plain images/text), so the snapshot does not include live buttons — same look, no taps mid-drag.

When `floating == false`, red X / green + / NavigationLink are live buttons.

---

## Android implementation guidance (Compose)

Any equivalent is fine if the **UX matches**:

1. **Preferred (parity with iOS):** On drag start, draw the row into a bitmap (`GraphicsLayer` / `drawToBitmap` / similar) and move that bitmap; hide the in-list row.
2. **Also acceptable:** Elevate the **entire row composable** (not just the grip) with `zIndex` + `graphicsLayer { translationY = … }` / `Modifier.offset`, while leaving a spacer of the same height in the list. Still must include X + text + + + grip in what moves.

**Not acceptable:** translating only the grip `Icon`.

Also:

- Gesture detector on grip only (`pointerInput` / `detectDragGestures` on the grip hit target ~30×44dp)
- Persist order on drag end
- Mirror layout for Grip Hand = Left (see `05-MASTER.md`)

---

## Favorites list

Same container pattern: full favorite row (name, date, chevron area as drawn) + grip floats as one unit — **no** red X / green + on Favorites rows.

---

## Checklist

- [ ] Drag starts only from grip
- [ ] Floating preview includes red X, name, green + (when present), grip
- [ ] In-list row disappears/hides while that item is dragging; slot height preserved
- [ ] Sibling rows stay put until release
- [ ] Release reorders list and saves
- [ ] Grip Hand Left still full-row float (mirrored chrome)
