# 03 — Menu (hamburger)

**Screenshot:** `04-menu.png`

## Presentation

- Modal / sheet over the app
- Title centered: **Menu**
- Leading control: **Done** (pill / rounded white button) — dismisses
- Background: light grey
- Body: white grouped list card with rounded corners
- Rows separated by hairline dividers

## Rows (exact order)

| # | Label | Trailing | Style / action |
|---|-------|----------|----------------|
| 1 | **Home** | none | **Blue** text; goes to Home and dismisses menu |
| 2 | **How-To Guide** | `>` | Opens How-To |
| 3 | **App Settings** | `>` | Same settings as gear |
| 4 | **Why I Built This App** | `>` | Story / about content |
| 5 | **Recent Searches** | `>` | Up to 20 prior search queries; tap re-runs / fills search |
| 6 | **App Info** | `>` | Version / info |
| 7 | **Contact** | `>` | Contact developer |
| 8 | **Benchmarks** | `>` | **Test / debug builds only** — omit from Play production |

## Android parity traps

- [ ] Missing rows (especially Why I Built This / Recent Searches / Contact)
- [ ] Home has a chevron or is black like others
- [ ] No Done to dismiss
- [ ] Benchmarks shown in production Play build
