# Cross Platform App — If I Must (Flutter / Dart)

**Purpose:** Docs for building a **cross-platform** If I Must (iOS + Android from one Flutter codebase) without rebuilding **deprecated / early** UX.

| Item | Location |
|------|----------|
| This pack (GitHub) | `Cross-Platform/` in [ifimustdocs](https://github.com/rbevis333/ifimustdocs) |
| This pack (Mac local) | `Web Apps/Cross Platform App/` |
| UI/UX source of truth | [`../Android-Parity/`](../Android-Parity/README.md) (current iPhone) |
| Live iPhone Swift | Mac Desktop `If I Must/` only |

---

## Start here

1. Read [`DO-NOT-BUILD.md`](DO-NOT-BUILD.md) — forbidden early/deprecated UI  
2. Read [`SOURCE-OF-TRUTH.md`](SOURCE-OF-TRUTH.md) — what to trust  
3. Follow [`START-GUIDE.md`](START-GUIDE.md) — phased build  
4. Stack notes: [`STACK.md`](STACK.md)  
5. Paste into a **new** Cursor agent: [`AGENT-PROMPT.md`](AGENT-PROMPT.md)  
6. Phase list: [`PHASES.md`](PHASES.md)

---

## Hard rule (avoid Android-style drift)

| Trust | Do not trust for layout |
|-------|-------------------------|
| `Android-Parity/` docs + screenshots (current iPhone) | Early `DESIGN_REFERENCE.md` / chip / profile designs |
| Shipping SwiftUI on Mac Desktop | Old chat ideas unless confirmed in Android-Parity |

If a Flutter agent proposes profile avatars, tag chips, or `d*`/`p*`/`u*` modifiers → **reject** and point at `DO-NOT-BUILD.md`.

---

## Parallel work

- **Windows:** finish native Android using `Android-Parity/`  
- **Mac:** Flutter using the **same** parity pack  
- Flutter follows **iPhone / Android-Parity**, not a drifted Android build
