# Contributing

This repository is a living **email OSINT** field manual. Fixes and additions are welcome.

## What to send

- Dead tools, moved URLs, or modules that no longer work
- Provider changes (for example a new MX fingerprint or a header field that disappeared)
- Techniques that stay on the lawful, passive side of the line
- Corrections to confidence rules, legal notes, or worked examples

## What we will not merge

- How to access accounts, complete password resets, or use leaked credentials
- SMTP probe / spray scripts
- Stealer-log marketplaces or password dumps
- Anything that helps stalk, harass, or impersonate a person
- Unsourced tool dumps with no investigative question

## How to edit

1. Fork the repo and branch from `main`
2. Keep the voice: field manual, not marketing
3. Prefer a concrete claim + source + caveat over another bullet list
4. Update the [checklist](docs/checklist.md) if you add a step people should always run
5. Open a pull request with a short “why”

## Style

- Original prose. Do not paste another site’s guide
- Name the tool, the question it answers, and the failure mode
- UTC dates on time-sensitive claims when you can
- Relative links between files in this repo

Questions: [hi@osintverse.com](mailto:hi@osintverse.com)
