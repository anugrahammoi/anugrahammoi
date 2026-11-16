# Anugraham — handoff (16 Nov 2026)

Paste or attach this file at the start of a new Claude chat, and commit it to the repo as `docs/HANDOFF.md` for Claude Code.

## What Anugraham is
A moi (gift) register for Indian family functions. Hosts and collectors record cash, UPI, gold, silver and gifts — offline at the hall, on several phones at once. Guests submit their moi from a web link / QR. Families see who gave what and what they gave back (reciprocity). Tamil + English at launch; built so any Indian language can be added. Android + web first, iOS later.

## Owner and constraints
- Owner/launcher: Jayanthy (Dependant's Pass, Singapore); fallback: a family member resident in India. Barani builds with Claude Code.
- Legal checks (EP/DP rules, employer contract) are parked until the app earns money. Keep it free and non-commercial until then.
- Essential costs approved (~₹15,000–30,000 INR in year 1); no ads budget. Timeline flexible.

## Where everything is (claude.ai artifacts)
- Master plan: https://claude.ai/artifact/NfNx7TjbB7edgR83jeEmnm
- MVP PRD (epics E1–E12, gap review §9): https://claude.ai/artifact/WBWjdw181N3HR6A5KK5EN9
- Architecture spec (flows, data model, rules, ADR-1..14, gap review §8): https://claude.ai/artifact/SqS42Rr3b37sTADrMkXu8v
- Design system (tokens, brand book): https://claude.ai/artifact/M6ZjeaeXYNCHfnxt38mWS8
- Mockups (42 screens, Tamil + English, mobile + web + print): https://claude.ai/artifact/AYW4QsFAbqAzigRipELgz8

## Decisions made
- Brand: Anugraham (அனுக்ரகம்). Store title "Anugraham – Moi & Gift Register".
- Stack: Flutter (BLoC, go_router, ARB l10n) + Firebase (Firestore, Functions TS, Storage, Hosting, App Check) in asia-south1; separate static guest web page; PDF/Excel generated on device.
- Never handle money: UPI deep links to the host's own UPI ID only. No automatic payment confirmation in v1.
- UPI receivers: family default + per-event override, up to 2 per event (bride's / groom's side); ₹1 test + name check.
- Guest flow: one form, one "Pay with UPI" button; entry saved before the UPI app opens; reference code like AN-K7Q2 in the UPI note; thank-you page. Auto-accept guest entries per event (default ON), totals split confirmed vs guest-reported; owner can reset the guest link.
- Roles: owner / collector / viewer; each person uses their own login; invite by single-use link or QR (7-day expiry). Up to 2 owners.
- Audit log and totals written only by Cloud Functions. No hard deletes. Money = int rupees, weight = int mg. ULID entry ids.
- Soft lock (late offline entries become pending). Serial numbers derived, frozen at lock.
- Languages: Tamil + English at launch; all Indian languages ready via ARB + region terminology packs.
- Notebook import (photo of old moi notebook): parked to the end; v1 = Excel/CSV import + photo-assisted entry; OCR later (Cloud Vision free tier), needs sample photos.

## Open decisions (waiting for Barani)
1. Firebase plan: Blaze recommended (Functions/Storage/phone auth need it); project is on Spark now.
2. Sign-in: Google only for MVP (recommended) vs phone OTP from day one.
3. Auto-accept guest entries default ON (with split totals) vs OFF.
4. Play developer account in whose name.

## Setup status (verified 16 Nov 2026)
- Firebase project: display name `anugraham-dev`, project ID `anugraham-dev-83368`, Firestore in asia-south1. Plan: Blaze (upgraded by Barani). Google sign-in enabled. No apps registered.
- GitHub repo: github.com/anugrahammoi/anugrahammoi (CLAUDE.md, HANDOFF, design tokens committed; PRD.md and ARCHITECTURE.md still to be exported into docs/).
- Not yet verified: local tools (Flutter 3.35+, Node LTS, Firebase CLI, FlutterFire CLI, `dart pub global activate very_good_cli`), domain anugraham.in availability, trademark search (Classes 9, 42).

## Next steps
1. Set a budget alert (₹500/month) if not done.
2. Put in the repo: `CLAUDE.md` (root), `docs/PRD.md`, `docs/ARCHITECTURE.md` (export both docs as Markdown), `docs/design-tokens.json`, `docs/HANDOFF.md`.
3. Claude Code session 1 prompt: "Read CLAUDE.md and docs/. Plan, then scaffold the monorepo per ARCHITECTURE §5.1: Flutter app with dev/prod flavors, packages, l10n (ta, en), go_router, theme from design-tokens.json, firebase emulators config. No features yet. Run analyze and tests."
4. Then: session 2 rules + tests, 3 auth/onboarding (E1, E12), 4 events/invites/QR (E2, E11), 5 hall entry (E3), then E6, E7, E5, E8, E4, E9.

## How Barani likes to work
Tanglish replies. Proactive product partner: analyse from product, business, design, engineering and security angles; surface gaps before building; separate facts, assumptions and recommendations; challenge ideas; avoid overengineering; ask only essential questions.
