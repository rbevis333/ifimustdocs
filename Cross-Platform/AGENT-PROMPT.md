# Agent prompt — Flutter If I Must (Mac)

Paste into a **new** Cursor Agent chat (do not rely on old Android Windows chats).

```text
You are building a Flutter (Dart) cross-platform app: "If I Must".

READ FIRST (required):
1. Cross Platform App/DO-NOT-BUILD.md
2. Cross Platform App/SOURCE-OF-TRUTH.md
3. Cross Platform App/START-GUIDE.md
4. Cross Platform App/STACK.md

UI/UX SOURCE OF TRUTH (current shipping iPhone — not early design):
- ../Android-Parity/README.md and its reading order
- ../Android-Parity/screenshots/*.png
- Especially: 05-MASTER.md, 08-REORDER-DRAG.md, 09-FONT-SCALING.md, HOW-TO-GUIDE.md, WHY-I-BUILT-THIS-APP.md

DO NOT use If I Must Design/DESIGN_REFERENCE.md or early chip/profile/modifier concepts.
DO NOT put Connections/Attributes above Word/Link/Make.
DO NOT build bare red X without filled circle; include green plus and reorder grip.
Trailing chrome is GEAR, not profile.

Live iPhone Swift (reference when behavior unclear):
/Users/randybevis/Desktop/If I Must/

Follow START-GUIDE.md phases. Start with Phase 0 (empty Flutter app running on iOS Simulator), then Phase 1 data model. Ask me before creating a GitHub repo or changing bundle IDs.

After each screen, compare to Android-Parity screenshots and checklist items.
```

---

## Short follow-ups you can paste later

**Home only**

```text
Implement Phase 2 Home only per Android-Parity/02-HOME.md and screenshots 01–03. No Master yet.
```

**Master only**

```text
Implement Phase 4 Master per Android-Parity/05-MASTER.md and screenshots 10–12. Connections list BELOW Word/Link/Make. Full row chrome: red filled X, blue name, green filled +, grip.
```

**Parity audit**

```text
Run Android-Parity/07-ANDROID-CHECKLIST.md against the Flutter app. List failures only; fix Master/Home first.
```
