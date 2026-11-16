# SOM1 Milestone 1: Foundation

Build only what is in this document. When every acceptance test passes, write the milestone report and stop. Do not start Milestone 2 or any feature from the master brief until the founder approves.

Product source of truth: `docs/SOM1_MASTER_BRIEF.md`. Engineering rules: `docs/ENGINEERING_RULES.md`. Read both in full before writing any code.

## Goal

A working, installable PWA with accounts, a secure database shaped for the whole product, privacy basics, and usage tracking. No companion chat yet. When Milestone 1 is done, later features slot in without rebuilding the foundation.

## Stack

Proposed stack. If any part conflicts with an existing THOQ standard, stop and ask before proceeding.

- **App:** Next.js (App Router) with TypeScript in strict mode.
- **Database, auth and storage:** Supabase (Postgres, Supabase Auth, Row Level Security, Storage). A new Supabase project for SOM1, created by DevOps.
- **Styling:** design tokens first (see "Design system"), then components.
- **PWA:** web app manifest plus a service worker, installable on Android, iOS and desktop.
- **Tests:** unit and integration tests, plus end-to-end tests for the auth flow.
- **Secrets:** environment variables only. Provide `.env.example` with every variable name and no values. Never ask for keys in chat; DevOps adds them.

## Scope

### 1. Project structure

Feature isolation from day one. Suggested layout:

```
src/
  app/                 routes and layouts
  core/                shared building blocks (db client, auth, config, usage tracking, errors)
  features/            one folder per product feature, empty placeholders allowed
    account/
  design-system/       tokens, theme, base components
supabase/
  migrations/          every schema change as a migration
  seed/                seed data (relationship catalogue)
docs/
tests/
```

A feature never imports another feature's internals. Shared logic goes in `core/`.

### 2. Design system before screens

The visual identity is not final. Build a replaceable token system and load it with the proposed palette from the master brief (Interface section), so the final identity can drop in later without touching screens.

- Tokens for colour, typography, spacing, radius, shadow and motion, in one place.
- Light and dark themes driven by the tokens.
- Base components only: button, input, card, sheet, toast, loading state, empty state.
- Mobile-first and responsive across phone, tablet and desktop.
- No sparkle, wand or robot icons anywhere.

### 3. PWA shell

- Manifest with app name SOM1, placeholder icons (simple, replaceable), theme colours from tokens.
- Service worker that caches the app shell and shows a friendly offline screen.
- Installable, and passes a standard PWA installability check.
- Structured so it can be wrapped as a native app later without rewriting (no browser-only assumptions in core logic; push notification handling isolated behind one module, not implemented yet).

### 4. Accounts

- Sign up, sign in, sign out, password reset.
- Email and password, plus Google sign-in.
- A `profiles` row created automatically for every new user.
- Account settings screen: display name, time zone, preferred language.
- `adult_confirmed_at` field on the profile, left empty. The 18+ gate itself is Milestone 2.

### 5. Database: the three-layer structure

Create the schema now, even for tables later milestones will fill. Every table has Row Level Security on, with policies so users can only ever read and write their own rows. Every change is a migration.

**Layer 1, the relationship**

- `relationship_types`: catalogue of relationships. Key, display name, sample line, featured flag, display order, active flag. Seed it with the 11 featured relationships, the dropdown list and Just Me & You (Neutral), exactly as listed in the master brief.
- `brains`: catalogue of AI brains. Key, friendly name, provider, description, active flag. Seed with placeholder rows for the four proposed brains; names are not final.
- `companions`: belongs to a user. Relationship type, custom relationship label (for Create Your Own), name, nickname, avatar reference, voice reference, brain, optional speciality (nullable; specialities are optional), persona settings as structured data, created and updated timestamps, archived flag.

**Layer 2, the understanding**

- `memories`: belongs to a user. Content (encrypted, see Privacy), category, life area (nullable), source (user stated, confirmed from chat, imported), confirmed flag, and `companion_id` nullable. Null means shared with all companions. A value means "just between us" with that one companion.
- `people`: the user's real people (name, relationship to the user, notes, key dates).
- `life_areas`: default and custom areas per user, with display order.

**Layer 3, care in action**

- `actions`: every external action SOM1 may take later. Type, payload, status with exactly these values: `proposed`, `confirmed`, `completed`, `failed`, `cancelled`. Nothing moves to `completed` without a prior `confirmed`. This is how the app tells talk apart from a finished action.
- `reminders`: belongs to a user and optionally a companion. Title, due time with time zone, recurrence, status.

**Cross-cutting**

- `consent_records`: type, subject, declaration text, method, timestamps. Used later for voice and likeness consent.
- `audit_log`: security-relevant events (sign in, export, delete, consent recorded).

Leave room for dating, shopping and subscription tables; do not build them now.

### 6. Privacy foundations

- Memory content and people notes encrypted at the application level before storage, with the key held in environment variables. Document the approach in `docs/ARCHITECTURE.md`.
- **Export my data:** one action that produces a downloadable file of everything stored about the user.
- **Delete my account:** removes the user and all their rows across every table, with a confirmation step. Log the event without keeping personal content.
- No user conversation content is ever sent anywhere for training.

### 7. Usage tracking

Every call SOM1 makes to an outside paid service will be recorded, starting now.

- `provider_rates`: provider, model, unit type (input token, output token, audio second, image, verification), price per unit stored as an integer in micro-units of the currency, currency code, effective-from date. Never floating point.
- `usage_events`: user, companion (nullable), feature, provider, model, units used per unit type, computed cost as an integer in micro-units, currency, request ID, timestamp.
- One shared function in `core/` that every provider call must go through, so usage can never be skipped.
- A provider adapter interface in `core/` that later brains will implement. Implement one adapter only, for a single test call, behind an admin-only route, so usage tracking can be proven end to end. No companion chat.
- An admin-only usage summary screen: cost by provider, model, feature and user, for a chosen date range.
- An admin-only account lookup: find a user by email and see their account status. This is the first piece of the back office described in the master brief.

### 8. Quality gates

- Pre-commit and pre-push hooks that run lint, type check and tests, and block em dashes and AI-attribution trailers in code, docs and commit messages (see `docs/ENGINEERING_RULES.md`).
- Security baseline: RLS on every table, server-side validation on every input, rate limiting on auth routes, secure headers, no secrets in the client bundle.

## Out of scope

Do not build any of these in Milestone 1, even partially: the opening experience and 18+ gate, relationship selection screens, chat, personas, memory learning, reminders UI, life areas UI, voice, multiple companion logic, brain selection UI, dating, shopping, food, subscriptions and payments, social media, image generation, notifications.

## Acceptance tests

Milestone 1 is done only when all of these pass and are shown in the report.

1. A new user can sign up with email, verify, sign in, sign out and reset their password.
2. A new user can sign in with Google.
3. A profile row is created automatically on sign-up.
4. User A cannot read or change any of User B's rows in any table (automated test across every table).
5. The app installs as a PWA on Android and desktop, and can be added to the iPhone home screen.
6. With the network off, the installed app shows the offline screen instead of a browser error.
7. Switching between light and dark theme changes every screen through tokens alone.
8. The relationship catalogue contains exactly the entries in the master brief, with the 11 featured ones in the right order.
9. Memory content is unreadable in the database without the application key.
10. Export my data downloads a complete file for a test user.
11. Delete my account removes every row for that user across all tables.
12. An action cannot go from `proposed` to `completed` without passing through `confirmed` (automated test).
13. The admin test call records a usage event with correct units and an integer cost calculated from `provider_rates`.
14. The admin usage summary shows that event, filterable by provider, model, feature, user and date.
15. A commit containing an em dash or an AI-attribution trailer is blocked by the hooks.
16. Lint, type check and all tests pass on main.

## Milestone report

When finished, write `docs/reports/MILESTONE_01_REPORT.md` with:

1. What was built, in plain language a non-technical founder can follow.
2. Each acceptance test with pass or fail, and how it was checked.
3. Screenshots of the key screens (sign up, sign in, account settings, offline screen, usage summary).
4. Anything that needs a founder decision, as a short numbered list.
5. Anything DevOps must set up (accounts, keys, domains), as a short checklist.
6. Known limitations.

Then stop and wait for the founder's approval. Do not begin Milestone 2.
