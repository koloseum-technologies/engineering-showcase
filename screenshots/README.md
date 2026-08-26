# Screenshots

Curated captures of completed Koloseum MVP services, taken from **locally seeded data** — never from the production database.

## Naming

```text
screenshots/
├── lounges/{account,operations,staff}/…
├── players/sessions/…
└── wireframes/players/sessions/…
```

Product captures are WebP. Sequence prefixes (`01-`, `02-`, …) follow the original flow order. Filenames are kebab-case.

The original export used `a` / `b` suffixes on otherwise identical mobile frames (theme or minor state variants). This curated set keeps the `a` frame of each flagship view.

## What is here

**Product UI (mobile viewport, 1082×2402 source, converted to WebP):** Lounge Account, Lounge Operations (dashboard, reputation, branch hub/select, front desk, inventory, eatery, settings), Lounge Staff, Player Sessions (home, find a lounge, start, single session, history).

**Wireframes:** desktop-oriented session flow (`01-select-lounge` … `06-checking-out`) plus home, find-a-lounge, and mobile layout/menu frames. These are the desktop-width artefacts in the set; the product PNGs were captured at a phone viewport. Recapturing Operations/Account/Staff at a desktop breakpoint remains outstanding — the wireframes and the mobile product captures together still show structure and density.

**Backroom** has no screenshots. `backroom-compliance` is a designated reference implementation; that gap is recorded in [`product/status.md`](../product/status.md).

## Personal data

Frames were audited before commit. Visible names, company details and Player IDs are seed fixtures (for example “Kinyanjui Gaming Ltd”, “KG Arena”, “MwangiK”, `KP0000001`), not production PII. The full uncurated export is kept only on the author’s machine and is not in git.

## Size

245 source files / ~56MB were cut to 32 curated files / ~2.3MB.
