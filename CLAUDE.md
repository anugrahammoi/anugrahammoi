# Anugraham — Claude Code guide

Anugraham is a moi (gift) register for Indian families: hosts and collectors record cash, UPI, gold, silver and gifts at functions; guests submit their moi from a web link; families see who gave what and what they gave back. Tamil first, every Indian language ready.

Read before any task: `docs/PRD.md` (what), `docs/ARCHITECTURE.md` (how), `docs/design-tokens.json` (look).

## Non-negotiables

1. **Never handle money.** No payment SDKs, no collecting funds, no commissions. UPI only as `upi://pay` deep links to the host's own UPI ID.
2. **Never lose an entry.** Every write must work offline. Clients never hard-delete; deletion = `status: 'deleted'` by an owner.
3. **Audit log and `events.stats` are written only by Cloud Functions.** Clients never write them.
4. **No hard-coded user-facing strings.** Every string goes in `app/lib/l10n/app_ta.arb` AND `app_en.arb`, same key, in the same change. Tamil uses the respectful form (நீங்கள்).
5. **Money is `int` rupees (`amountRupees`), weight is `int` milligrams (`weightMg`).** Never `double`.
6. **Minimum personal data.** Phone numbers optional. Never log names, phone numbers, amounts or UPI IDs.
7. **Never judge amounts in UI or copy** (no rankings, no "top givers").

8. **Lock is soft.** After an event is locked, late offline entries arrive as `status: 'pending', late: true` — never rejected or dropped.
9. **No stored serial numbers.** Order comes from `deviceTime`; numbers are frozen only when the event is locked.
10. **Guest and exported text is untrusted.** Escape it on render; prefix cells starting with `= + - @` in CSV/Excel exports; strip EXIF from photos before upload.

## Stack

- Flutter stable (Dart 3.9+), Android + iOS + web, in `app/`
- State: `flutter_bloc` (Cubit by default, Bloc when events matter). Routing: `go_router`. i18n: `flutter gen-l10n` with ARB.
- Packages in `packages/`: `anugraham_ui` (design system), `domain_models`, `firestore_client`, `*_repository`.
- Layers: widgets → bloc → repository → data. Widgets never import Firestore; blocs never see DTOs.
- Firebase (region asia-south1): Auth (Google sign-in only in MVP; phone OTP later), Firestore with offline persistence, Storage, Functions 2nd gen in TypeScript (`functions/`), Hosting, App Check.
- Guest page: static HTML + minimal JS in `guest-web/` (no Flutter, keep it under ~100 KB).
- PDF/Excel: generated on device (`pdf`, `printing`), Noto Sans Tamil embedded.
- Entry IDs: ULID generated on the device.

## Design

- Use only tokens from `docs/design-tokens.json` via `anugraham_ui` ThemeExtension. No raw hex colours, font sizes or paddings in feature code.
- Body text 17px, touch targets ≥ 56px in hall mode (≥ 48px elsewhere), contrast ≥ 4.5:1.
- States never rely on colour alone: confirmed = green + tick + label; pending/offline = amber + icon + label.
- Layouts must survive text 40% longer than English. Use `EdgeInsetsDirectional` (start/end), never left/right.
- Mockups: "Anugraham – MVP Mockups" canvas. Match them; ask before inventing new screens.

## Working rules for Claude Code

- Plan first, then implement one epic (see PRD build order) per session. Keep PRs small.
- Write tests with the code: unit + bloc tests for logic, widget tests for screens (Tamil and English), Firestore rules tests (allow AND deny) for every rule change.
- Before saying done: `dart format`, `flutter analyze` (zero warnings), `flutter test`, and `npm test` in `functions/` must pass.
- Never commit secrets, `google-services.json` for prod, or `.env` files. Use the dev Firebase project unless told otherwise.
- Never deploy to prod, change Firestore location, or run destructive Firebase commands without explicit approval.
- Ask when a requirement is unclear rather than guessing — especially anything touching money, deletion, privacy or roles.

## Commands

```bash
# app
cd app && flutter pub get && flutter gen-l10n
flutter analyze && flutter test
flutter run -d chrome            # web
flutter run                      # device / emulator

# functions + rules
cd functions && npm ci && npm test
firebase emulators:start --only auth,firestore,functions,storage
firebase deploy --only firestore:rules --project anugraham-dev-83368
```
