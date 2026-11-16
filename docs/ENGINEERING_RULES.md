# SOM1 Engineering Rules

These rules apply to every milestone. They follow the THOQ Engineering Build Standard.

## How work proceeds

1. `docs/SOM1_MASTER_BRIEF.md` is the product source of truth. Do not change product decisions in it. If something in the brief seems wrong or unclear, list it in the milestone report as a question for the founder.
2. Build only the current milestone document. Never build ahead, even "just the structure" of a later feature, unless the milestone document says so.
3. One feature, or one necessary connected set, at a time. Finish it, test it, report it, then stop and wait for the founder's go-ahead.
4. Anything under "Still open" in the brief is not a requirement. Use placeholders and flag it.
5. When a decision is needed that the documents don't answer, pick the simplest reversible option, record it in `docs/DECISIONS.md`, and flag it in the report.

## Code and repository standards

- No references to any AI coding tool or assistant anywhere: code, comments, docs, commit messages, branch names, file names. No AI-attribution trailers in commits. (AI providers that power SOM1's brains are product features and may be named in provider adapters, configuration and the privacy policy.)
- No em dashes anywhere in code, copy, docs or commits.
- No sparkle, wand or robot icons in the interface.
- Money is never floating point. Store amounts as integers in the smallest unit (micro-units for provider rates and usage costs, minor units for prices).
- Commit to main only. Small, clear commits written in plain English.
- Pre-commit and pre-push hooks enforce lint, type check, tests, the em dash ban and the attribution-trailer ban.
- Design system before screens: every colour, font, spacing and radius comes from tokens.
- Feature isolation: each feature lives in its own folder and never reaches into another feature's internals.
- Security baseline: Row Level Security on every table, server-side validation, rate limiting on sensitive routes, secure headers, no secrets in the client.
- Secrets live in environment variables only. Keep `.env.example` current. Never request keys in chat; DevOps sets them.
- Every paid outside call goes through the shared usage-tracking function.
- Every external action uses the `proposed`, `confirmed`, `completed` path. Nothing completes without user confirmation.

## Documentation

Keep these four handover documents current at the end of every milestone:

1. `README.md`: what SOM1 is, how to run it locally, how to run tests.
2. `docs/ARCHITECTURE.md`: structure, data model, the three layers, encryption, usage tracking, provider adapters.
3. `docs/OPERATIONS.md`: environments, environment variables, deployment, backups, third-party accounts.
4. `docs/DECISIONS.md`: dated log of technical decisions and why.

Each milestone also produces `docs/reports/MILESTONE_XX_REPORT.md`.
