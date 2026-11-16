# Moi App – MVP PRD (v1)

Nov 7, 2026 · @Barani

## 1. Overview

**Goal:** a free MVP that lets a Tamil family and their helpers record every moi (cash, UPI, gold/silver, gifts) for a function in one shared list, during the function and from guests' phones, then print a clean register and remember what to return later.

**Platforms:** Android app + web (the web version also covers iPhone users until a native iOS app). Tamil + English.

**Principles**

- Faster than writing in a notebook, even with no network.
- The app never touches money — UPI goes guest → host directly.
- Tamil first, not translated as an afterthought.
- Collect the minimum personal data (DPDP).

**Assumptions**

- *Assumption:* B2C MVP first, with 2–3 professional moi writers as design partners.
- *Assumption:* stack per master plan — Flutter app, a small separate guest web page, Firebase (Mumbai region).
- *Assumption:* free during MVP; no payments or subscriptions in v1.

## 2. Brand name shortlist

None of these has been checked yet. For each, check: Google Play / App Store search, a `.in` / `.app` domain, Instagram handle, and IP India trademark search (Classes 9 and 42).

| Name | Meaning | Pros | Cons |
| --- | --- | --- | --- |
| **Vazhthu** (வாழ்த்து) | Blessing / wishes | Warm, emotional, fits every function | Tamil-only meaning |
| **Panthal** (பந்தல்) | The function pandal | Instantly evokes functions; short | May be used by event/decor businesses |
| **Anbalippu** (அன்பளிப்பு) | Gift of love | Clear meaning | Long to say and type |
| **MoiPetti** (மொய் பெட்டி) | Moi box | Says what it does | Close to existing "Moi" names; weaker trademark |
| **Thaambulam** (தாம்பூலம்) | Betel-leaf offering tray | Cultural, distinctive | Spelling varies; harder for other states |
| **Nenaivu** (நினைவு) | Remembrance | Matches the reciprocity promise ("never forget who gave") | Abstract |

**Recommendation:** pick a distinctive brand and keep "moi" in the store title and tagline for search. For other states, use the same brand with that region's term in the tagline (shagun, muyi, aaher).

**Proposed by Barani (8 Nov 2026): Anugraha** (அனுக்ரகம் — grace / blessing)

- *For:* positive meaning, understood across South India and in Hindi (Sanskrit root) — good for pan-India.
- *Against:* a very common name — many businesses and NGOs already use it, including an IT company (Anugraha Info Systems Pvt Ltd, Karnataka); an "ANUGRAHA" trademark was filed in Aug 2026 (Class 17, plastic pipes — not software, so not a direct block); an iOS app called "Anugrah" (textile/finance) exists. A plain common word is weak as a trademark and hard to find in store search.
- *How to make it work:* register a composite mark (logo + name, e.g. **Anugraha Moi**), always pair it with the descriptor in the store title — *Anugraha – Moi & Gift Register* — and run a proper IP India search in Classes 9 and 42 before filing. Check domain and Instagram handle availability.
- In Tamil speech the common form is *Anugraham*; decide one spelling for the logo and use it everywhere.

## 3. Personas

| Persona | Context | Needs |
| --- | --- | --- |
| **Host parent** (e.g. 50s, organising a daughter's wedding) | Mid-range Android, WhatsApp-fluent, prefers Tamil | One trusted total; a printed register; know what to return later; no family disputes |
| **Family collector** (cousin at the moi table) | Crowd, noise, bad network, 3–4 hours of entries | Speed, big buttons, no lost entries, name suggestions |
| **Professional moi writer** | 20–50 functions a season, often in Pudukkottai/Thanjavur belts | Many events, fast entry, printed receipts, reputation for accuracy |
| **Guest** | Opens a WhatsApp link or scans a QR on the invitation | Pay with GPay/PhonePe in two taps; be sure the family knows it was them |

## 4. Epics, user stories and acceptance criteria

### E1 — Account, language, family

- As a user I sign in with Google or phone OTP and choose Tamil or English on first launch.
- As a host I create a *family* that owns events and contacts.
- **Done when:** language can be switched any time; every screen has Tamil strings; the user can delete their account and data from Settings.

### E2 — Events and collectors

- As a host I create an event: type (from a configurable list: marriage, kaathu kuthu, mottai, manjal neerattu, griha pravesam, seemantham, other), date, venue, host names.
- **UPI receivers (user decides):** a family can save a *default UPI ID*. Each event uses the family default unless the host overrides it. An event can have **up to 2 receivers** (e.g. Ponnu veedu / Maapillai veedu), each with a display name and UPI ID; the guest picks which side they're giving to.
- On saving a UPI ID, the app asks the host to check the name: it opens their UPI app with the ID (no amount), the UPI app shows the bank-registered payee name, and the host confirms Yes/No back in Anugraham. Fallback: a relative scans the QR and sends ₹1. (A ₹1 payment to yourself is not used — UPI apps may block paying your own account. The app cannot verify UPI IDs itself without a payment provider.)
- As a host I invite collectors by link/QR with a role: Owner, Collector (add entries), Viewer.
- **Done when:** a collector joins in under 1 minute; UPI ID format is validated; changing the family default doesn't change events already shared; roles are enforced by security rules, not just the UI.

### E3 — Hall entry mode (core)

- As a collector I add an entry: giver name → ooru → relation (optional) → amount → mode (cash / UPI / gold / silver / gift).
- Name field suggests matching people from earlier entries and contacts; warns on a likely duplicate in this event.
- Running total, count, and "my entries" visible all the time.
- **Done when:** a trained collector logs an entry in ≤ 10 seconds; airplane-mode entries sync later with zero loss; 4 collectors entering at once never overwrite each other; the owner can *lock* an event so it becomes read-only.

### E4 — Guest link / QR (web)

- As a host I share an event link on WhatsApp, or print its QR on the invitation or a hall standee (see E11).
- **As a guest I see one form and one button:** choose bride's / groom's side, enter name, ooru, amount, optional wishes → tap **"Pay ₹X with UPI"**. The entry is saved at that moment, then my UPI app opens with the host's UPI ID, the amount and a note like `AN-K7Q2 Suresh`.
- When I come back, I see **"Thank you — your moi is recorded"** with the reference code and a small *"Payment didn't go through? Try again"* link. No extra questions. If I close the page, nothing is lost.
- **Auto-accept (default ON, per event):** guest entries count in the total with a *"guest-reported"* tag. With auto-accept OFF, they wait as **Pending** until the host confirms or rejects; the UPI note code makes matching with the bank/UPI history easy.
- Host can **add from UPI history** if someone paid but never filled the form.
- **Done when:** the page loads in < 2 seconds on 4G; works without the app or login; spam is limited (App Check + rate limit); the page clearly says the app never receives the money.
- **Not in v1:** automatic payment confirmation. That needs a licensed payment gateway with host KYC — possible later as optional "Verified payments".

### E5 — Gifts

- As a collector I log a non-cash gift with description, optional photo, optional weight (gold/silver, grams) and optional estimated value.
- **Done when:** photos are compressed (≤ 300 KB) and stored privately; gifts appear in totals separately from cash.

### E6 — Manage

- List with search and filters (mode, ooru, relation, collector, status); edit; soft delete with a reason.
- Every edit/delete is in an audit log visible to Owners ("who changed what, when").
- Totals by mode and by collector.
- **Done when:** search returns in < 1 second for 2,000 entries offline.

### E7 — Register export

- PDF "moi register" in Tamil or English: event header, serial number, name, ooru, amount/gift, totals; sort by serial, name or ooru.
- Excel/CSV export; share via WhatsApp.
- **Done when:** Tamil prints correctly on A4 and on a phone screen; a 1,000-entry register generates in < 10 seconds.

### E8 — Reciprocity ledger

- People are de-duplicated into *contacts* (name + ooru + optional phone), with manual merge.
- As a host I can also record moi *I gave* at someone else's function.
- A contact page shows everything received from and given to that person/family, newest first.
- **Done when:** opening a contact shows both directions; when entering a name at a new event, the collector sees what we last gave/received from them.

### E9 — Privacy and trust (applies everywhere)

- First-run consent notice in Tamil/English; guest page notice.
- Export my data; delete my data; grievance contact in Settings.
- Guest phone numbers optional, never required.

### E10 — Notebook import (beta)

- As a host I photograph or upload pages of an old moi notebook (camera, gallery or PDF; many pages at once) — on mobile or desktop.
- I can also import an Excel/CSV list (for families who already keep a sheet).
- The app reads each page (handwritten Tamil or English) and extracts rows: name, ooru, relation, amount or gift.
- **Review step is mandatory:** the page photo and the extracted rows side by side; unsure cells highlighted (`pending` colour + icon); I correct, delete or add rows. Nothing is saved without my confirmation.
- I attach the import to a **past event** (e.g. "1998 — elder brother's wedding") and say whether it's moi **received** or moi **we gave**; rows link into contacts and the reciprocity ledger.
- **Done when:** a clear page of 25 rows is reviewed and saved in under 3 minutes; the original photos are kept privately with the event (and can be deleted); the user is told before upload that the photo is processed by an AI service.
- **Gate before building:** test extraction accuracy on 10+ real notebook pages (different handwriting, ink, paper). If accuracy is poor, ship Excel/CSV import first and keep photo import as "coming soon".

**Status (8 Nov 2026):** parked — revisit at the end, after sample notebook photos are available. **Cost plan (free first):** v1 = Excel/CSV import + *photo-assisted entry* (page photo on top, fast entry form below, no OCR — zero cost, zero accuracy risk); v1.5 = Google Cloud Vision handwriting OCR within its free 1,000 pages/month + rule-based row parsing + the review step; beyond the free quota it becomes a paid "notebook scan credits" feature. Avoid free LLM tiers that may use uploaded content to improve their products — these pages hold relatives' names and amounts.

### E11 — QR codes

- **Share link & QR sheet** (mobile + web): WhatsApp share, copy link, save QR image, auto-accept switch.
- **Invitation QR card** (print-ready PNG/PDF, Tamil + English) for the invitation printer.
- **Hall table standee** (A5 PDF, Tamil + English, 3 steps: scan → name & amount → pay).
- **Add collector by QR:** owner shows a single-use QR (expires in 7 days) with a role; the other person scans with their own phone and signs in with their own account.
- QR always encodes the guest page link (`anugraham.in/g/{token}`), never a raw `upi://` link — one QR serves both receivers and records the guest's details. Generated on the device (free).
- **Done when:** a printed QR at 3 cm scans from 30 cm with a mid-range Android camera; join QR can't be reused.

## 5. Non-functional requirements and out of scope

**Non-functional**

- Offline-first: every write works without network; sync on reconnect.
- Runs on Android 8+ phones with 2–3 GB RAM; app download < 30 MB.
- Guest page first load < 200 KB.
- Data in Firebase asia-south1 (Mumbai); daily backup export.
- Accessibility: large text mode; high-contrast buttons for older users.
- Analytics only after consent; no ads; no data selling.

**Out of scope for v1**

- Processing or holding money; commissions.
- Voice entry (planned v1.5). Notebook import moved into MVP as E10 (beta).
- Native iOS app (web covers iPhone users for now).
- Translations beyond Tamil + English at launch — but the code is multilingual-ready from day 1: ARB strings, Noto fonts for every Indian script, layouts that survive 40% longer text, start/end (RTL-safe) spacing, and a per-region terminology pack (moi / muyi / chadivimpulu / aaher / chandlo / shagun).
- Paid plans, B2B billing, printer integration.

## 6. Data model, screens and build order

### Data model (Firestore)

| Collection | Key fields |
| --- | --- |
| `users` | uid, name, phone?, language, createdAt |
| `families` | id, name, ownerUid, defaultUpi? {vpa, displayName, verifiedAt?} |
| `families/{id}/members` | uid, role (owner / collector / viewer) |
| `families/{id}/contacts` | id, name, nameTa?, ooru, relation?, phone?, mergedInto? |
| `events` | id, familyId, type, title, date, venue, hostNames, receivers\[0..2\] {id, label, vpa, displayName} (copied from family default at creation, editable), status (open / locked), createdBy |
| `events/{id}/members` | uid, role |
| `events/{id}/entries` | id (device-generated), contactId, giverName, ooru, relation?, mode, amount?, giftDesc?, weightGrams?, photoRef?, receiverId?, status (confirmed / pending / rejected / deleted), source (hall / guest), utr?, createdBy, deviceTime, serverTime |
| `events/{id}/auditLog` | entryId, action, before, after, by, at |
| `outgoing` (per family) | contactId, eventName, date, mode, amount / gift |
| `guestSubmissions` | eventId, receiverId, payload, ipHash, createdAt → moved into entries as pending |

### Screens (for mockups)

1. Language pick + sign-in
2. Home: my events + "New event"
3. Create event
4. Event dashboard: totals, collectors, share link/QR, lock
5. **Hall entry** (the most important screen)
6. Entry list + filters
7. Entry detail / edit / audit trail
8. Pending guest submissions
9. Gift entry with photo
10. Contact (reciprocity) page
11. Export register preview
12. Settings: language, privacy, delete data; plus Notebook import — capture (mobile) and review (desktop)
13. **Guest web page** (pay + submit)

### Build order in Claude Code

Save this PRD as `docs/PRD.md` in the repo. One epic per session; ask Claude Code to plan first, then build with tests.

1. Repo + CLAUDE.md + Flutter skeleton + i18n (Tamil/English) + Firebase dev project
2. E1 sign-in, language, family
3. E2 events and roles + security rules
4. **E3 hall entry with offline sync** (test with airplane mode and 4 devices)
5. E6 manage + audit log
6. E7 PDF/Excel export
7. E5 gifts with photos
8. E8 reciprocity ledger
9. E4 guest web page
10. E10 notebook import (beta, after the accuracy test), then E9 privacy pass, then closed testing on Play

## 7. Success metrics and open questions

**MVP success (first season)**

- 20 real events recorded end-to-end.
- 3+ events where the collector used hall mode with no paper backup.
- Median entry time ≤ 10 seconds.
- 30%+ of hosts share the guest link.
- 2+ moi writers ask to keep using it.

**Open questions**

1. ~~Brand~~ **Decided (8 Nov 2026): Anugraham** (அனுக்ரகம்). Store title: *Anugraham – Moi & Gift Register*. Still to do: IP India search (Classes 9, 42), domain, Instagram handle.
2. ~~UPI ID per event or per family?~~ **Decided:** family default + per-event override, up to 2 receivers per event (see E2).
3. Guest submissions need host confirmation; collector entries are confirmed directly. (Proposed — confirm.)
4. Owner of the Play developer account: Jayanthy, or a family member resident in India? (Decide before Play closed testing.)
5. Which 2–3 moi writers become design partners?

## 8. Sequence and setup before Claude Code

### Sequence (in Claude chat, before any code)

1. **Design system** — Anugraham colours, Tamil + English fonts, spacing, buttons. Every mockup and the Flutter theme reuse it.
2. **Mockups** — the 13 screens in Section 6, hall entry and guest page first.
3. **Clickable prototype** — link the key screens; show it to 3–5 people (2 moi writers, 2 hosts) and fix what confuses them. Cheapest place to find mistakes.
4. **Architecture spec** — system diagram, offline sync design, Firestore security-rule model, repo/folder structure, key decisions (state management, routing), and the first `CLAUDE.md`.
5. **Design handoff** — spacing, states, edge cases per screen, so Claude Code doesn't guess.
6. **Switch to Claude Code** — repo scaffold, then epics in the build order above.

### Accounts (one-time)

- [ ] GitHub — private repo `anugraham`
- [ ] Google account for the project (not your work account)
- [ ] Firebase: two projects, `anugraham-dev` and `anugraham-prod`; Firestore location **asia-south1** (can't be changed later); Blaze plan with a budget alert (e.g. ₹500/month)
- [ ] Claude Code installed and logged in
- [ ] Later (before beta): Google Play Console in the owner's name; domain

### Local machine

- [ ] Git
- [ ] Flutter SDK (stable; Flutter 3.35+ / Dart 3.9+ so the Dart MCP server works)
- [ ] Android Studio + one emulator (and one real low-end Android phone for testing)
- [ ] Node.js LTS (Firebase CLI, Cloud Functions, guest web page)
- [ ] Firebase CLI + FlutterFire CLI
- [ ] VS Code with Dart/Flutter extensions
- [ ] Tamil fonts for PDF: Noto Sans Tamil (or Mukta Malar)

### Claude Code plugins / MCP servers

| Plugin | Why | Notes |
| --- | --- | --- |
| **firebase** (Google) | Lets Claude Code manage Firestore, Auth, rules, hosting directly | Official partner plugin |
| **github** (GitHub) | Issues, PRs, repo actions | Official partner plugin |
| **code-review** (Anthropic) | Multi-agent PR review before merging each epic |  |
| **Very Good AI Flutter Plugin** | Flutter skills (architecture, testing, i18n, theming), the Dart MCP server, auto analyze/format hooks | Community plugin — review before installing. Uses BLoC; pick **one** Flutter skill pack only so conventions don't clash |

### Skills to use here in chat (already available)

- *design:user-research* — interview guide for moi writers and hosts
- *design:ux-copy* — Tamil/English button and error text
- *design:design-critique* and *design:accessibility-review* — check mockups (older users, big text)
- *design:design-handoff* — specs for Claude Code
- *product-management:synthesize-research* — turn interview notes into decisions

## 9. Gap review (16 Nov 2026)

A pass over pages, product and rules after the mockups. **P0** = needed for MVP, **P1** = before public launch, **P2** = later.

### 9.1 Pages

| Page / state | Status | Priority |
| --- | --- | --- |
| Welcome + sign-in (language, Google, join by QR, legal line) | Mocked (17) | P0 |
| First-time setup + consent (name, family name, what we store, optional analytics) | Mocked (18) | P0 |
| Home — my events, events I help with, bottom navigation | Mocked (19) | P0 |
| Accept invite (role, can / can't, signed-in account) | Mocked (20) | P0 |
| Entry detail + edit + change history | Mocked (21) | P0 |
| Delete account (what goes, what stays, export first, web link) | Mocked (22) | P0 |
| People list + search; "Moi we gave" add form | Spec only — reuse list and form patterns | P0 |
| Manage members (see who joined, change role, remove) | Spec only | P0 |
| Lock event confirmation (warn about unsynced collectors) | Spec only | P0 |
| Empty, loading, offline and error states for every list and form | Spec only — one shared pattern | P0 |
| Sign-out warning when entries are not yet synced | Spec only | P0 |
| Privacy policy, Terms, web account-deletion page, landing page (anugraham.in) | Not started — static pages | P1 |
| Force-update screen (app version too old) | Spec only | P1 |
| Help / contact us (WhatsApp or email) | Spec only | P1 |
| Phone OTP screens | Deferred with phone sign-in | P2 |
| Notebook import | Parked (E10) | P2 |

### E12 — Sign-in, onboarding and account (new)

- **Sign-in: Google only in MVP.** Assumption: almost every Android phone already has a Google account, it costs nothing and has no SMS-fraud risk. Phone OTP comes later if real users ask (needs the paid Firebase plan and SMS-abuse protection).
- First run: pick language → sign in → name + family name + consent → Home. A person who arrives through an invite link or QR skips family creation and lands on *Accept invite*.
- **Sample event:** "Look at a sample event first" opens a read-only demo with fake data, so a host can try hall entry before the real function.
- A family can have **two owners** (e.g. both parents, or host + spouse) so the data survives a lost phone or account. Ownership can be transferred.
- Sign-out is blocked (with an explanation) while entries are waiting to sync.
- Delete account: in the app **and** on a web page; export offered first; events with another owner continue with that owner.
- Users must be 18 or older (stated at sign-in and in the Terms).
- **Done when:** a new user reaches hall entry in under 2 minutes; an invited collector reaches hall entry in under 1 minute; deleting an account removes its data within 30 days.

### 9.2 Product rules that were missing

| Gap | Decision (assumption unless you change it) | Priority |
| --- | --- | --- |
| Serial numbers can't be assigned while 4 phones are offline | No stored serial number. Order = time of entry; numbers are shown in lists and frozen into the PDF when the event is locked | P0 |
| Entry made offline, then the owner locks the event before it syncs | Lock is soft: late entries arrive as *Pending — late* for the owner to accept. Never dropped | P0 |
| Same guest recorded twice (two collectors, or collector + guest link) | Warn on same name + ooru + amount within the event; owner can merge. Never auto-delete | P0 |
| Envelope with no name | One-tap "No name" (பெயர் இல்லை) entry; can be fixed later | P0 |
| Cash handover at the end of the function | Dashboard shows **cash per collector in rupees** ("Ravi: ₹72,000 to hand over"), not just entry counts. Biggest real pain at the moi table | P0 |
| Guest link abused to inflate totals (auto-accept is ON) | Totals show *confirmed* and *guest-reported* separately; PDF marks guest-reported rows; owner can bulk-reject; owner can **reset the guest link** | P0 |
| Tamil vs English spelling of the same person (முருகன் / Murugan) | v1: suggestions match within the same script only; manual merge of contacts. Transliteration matching later | P1 |
| Two families at one wedding | One event with two receivers (already designed); register PDF can be exported per side | P0 |
| Host wants to know a guest entry arrived | In-app badge in v1; push notifications later | P1 |
| Feedback from real users | "Tell us" link in Settings + one question after an event is locked | P1 |
| NRI guests (no Indian UPI) | Guest page says cash / bank transfer to the family directly; no foreign payments in v1 | P2 |

### 9.3 Decisions needed from you

1. **Firebase plan:** the free Spark plan cannot run this design — see Architecture §8.1. Recommendation: Blaze with a small budget alert.
2. **Sign-in:** Google only for MVP (recommended), or phone OTP from day one?
3. **Auto-accept guest entries default:** keep ON with the split totals above, or default OFF?
