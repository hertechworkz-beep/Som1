# SOM1 Hard Rules

These rules come from the founder. They are not preferences. They apply to every milestone and are followed to the letter. If any rule cannot be met, stop and say so before continuing.

This file sits beside `docs/ENGINEERING_RULES.md` and carries the same weight.

## The codebase reads as careful human work

- Nothing in the repository names or hints at an AI tool: code, comments, docs, the README, commit messages, branch names and file names.
- No filler. No comments that narrate obvious code, no leftover placeholders or TODOs, no dead code, no generic boilerplate, no emoji in code, comments or commits.
- Comments explain why, where a reader would otherwise wonder. Names are clear and consistent across the whole codebase.
- One formatter and one lint setup for everything. Code is spaced and arranged so related lines sit together and a reader can follow a file from top to bottom.
- The README and docs are written the way a small, careful product team writes them.

## Solid and modular

- Build step by step. Every feature is built to production quality before the next one starts. No half-built features and no shortcuts to be fixed later.
- Each feature lives in its own folder with a short README: what it does, the data it owns, and the interface other features may use.
- Features talk to each other only through those interfaces. Fixing one feature must never break another, and each feature has its own tests that run on every push.
- Two developers must be able to work on two different features at the same time and merge cleanly into one app. Keep shared files few and small, let each feature register itself instead of editing a central list, and timestamp migrations.
- The backend is as tidy as the frontend: consistent structure, clear error handling, clear naming, nothing mechanical or repetitive.
- A human developer opening the repository for the first time should be able to find any feature in under a minute and understand it from its README.

## Admin

- The admin area is its own separate space: its own routes, layout, access checks and folder. It is never mixed into user-facing features.
- It is built to the same standard of neatness and design as the user-facing product.

## Ready for native

- Every milestone keeps the product easy to convert to a native app. Core logic makes no browser-only assumptions.
- Anything that differs by platform (push notifications, storage, camera, microphone, sharing, install) sits behind a small adapter.

## Look, feel and experience

- No generic AI-generated look: no template layouts, no default component-library styling left as is, no sparkle, wand or robot icons, no stock gradients.
- Vibrant and warm, never dull. Aesthetics, UI and UX are core requirements, not polish.
- Interactive, with purposeful animation: transitions, message arrivals, button feedback and small moments of delight. Honour the reduced-motion setting, and never let animation slow the app down.
- Users should feel at ease and at home every time they open the product.
- Every screen has designed loading, empty and error states.
- Sign-up asks for the minimum. Anything more is collected afterwards.
- New users get a short, friendly, skippable welcome.
- Users are invited to install the app, with step-by-step directions for iPhone (Safari, Share, Add to Home Screen) and for Android.
- Settings are robust: every feature ships with the settings a user needs to shape it to their liking.
- Help is an extensive, interactive, searchable Q&A.
- Legal documents are written thoroughly and solidly before they go to lawyers.
- Screens are built from the approved designs. Where no design exists yet, stop and ask instead of inventing one.

## Additions to Milestone 1

These are added to `docs/MILESTONE_01_FOUNDATION.md`.

Scope:

- An install guide inside the app: an install prompt where the browser supports one, and clear step-by-step directions for iPhone and Android.
- The admin usage summary and account lookup live in a separate admin area, as set out above. Only accounts marked as admin can open it.
- Every feature folder created in this milestone has its short README.

Acceptance tests, in addition to the existing ones:

- The in-app install guide shows the correct steps on iPhone and on Android.
- A signed-in user who is not an admin cannot open any admin route (automated test).
- Every feature folder has its README, and no feature imports another feature's internals (automated check).
- Sign-up asks only for what is needed to create the account.
