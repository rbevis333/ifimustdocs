# STACK — Flutter / Dart recommendations

Not mandatory forever — pick sensible defaults, document choices in the project README, and don’t churn stacks mid-Master.

## Core

| Area | Recommendation | Notes |
|------|----------------|-------|
| UI | **Flutter 3** + Dart 3 | One codebase → iOS + Android |
| State | **Riverpod** or **flutter_bloc** | Pick one; Riverpod is fine for this app size |
| Navigation | **go_router** | Matches stack: Home → lists → Master |
| Local DB | **Drift** (SQLite) | Close to relational graph; good for backup JSON |
| Paths / prefs | `path_provider`, `shared_preferences` | Settings flags (Hints, Symbols, …) |
| Icons | `cupertino_icons` + custom Material/Cupertino mix | Prefer filled-circle X/+ look from parity screenshots |
| URLs | `url_launcher` | How-To YouTube link |
| Contacts export | `flutter_contacts` or platform channels | Master “Phone” export |
| Call / SMS / email | `url_launcher` (`tel:`, `sms:`, `mailto:`) from **buttons**, not blue field text |

## Billing (later phases)

| Platform | Package direction |
|----------|-------------------|
| iOS | `in_app_purchase` / StoreKit wrapper |
| Android | same plugin → Play Billing |

Match iPhone product: yearly + trial when you wire paywall.

## Explicit non-goals for v1 Flutter

- CloudKit sync (iPhone-only for now; Flutter stays offline-first + JSON backup)  
- Recreating Android Compose code line-by-line  
- Using early design PNGs as the component library  

## Theming

- Define CSS-like tokens in Dart (`AppColors`, `AppRadii`, `AppFontScale`) from `Android-Parity/01-VISUAL-SYSTEM.md` and `09-FONT-SCALING.md`  
- Avoid Flutter “purple demo” defaults on Home/Master  
- Light default; Dark from Settings → Theme  

## Testing

- Widget tests for parsers (Add / Word / Search)  
- Golden tests optional for Home/Master against screenshots later  

## Repo strategy

- Flutter code: **new git repo** when you’re ready (ask before creating GitHub remote)  
- Docs: stay in `Cross Platform App/` + `ifimustdocs/Cross-Platform/`  
- Do not put Flutter inside the Xcode project folder
