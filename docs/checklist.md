# Email OSINT Investigation Checklist

Print this or copy it into your case file. One checklist per address. If a second email appears, start a new sheet and cross-link the IDs.

Back to the [Email OSINT Guide 101](../README.md).

---

## Case header

```text
Case / ticket: ________________________________
Analyst: _____________________________________
Lawful basis / authorization: _________________
Date opened (UTC): ____________________________
Primary address (exactly as received): _________
Normalized forms:
  - lowercase: ________________________________
  - plus-stripped: ____________________________
  - Gmail-dotless (if consumer Gmail): _________
Classification:  [ ] consumer  [ ] corporate  [ ] role
                 [ ] disposable  [ ] catch-all  [ ] alias
                 [ ] spoofed  [ ] unknown
Name hypothesis (label as hypothesis): _________
Do-not-do list: no login, no reset, no credentials, no contact
```

---

## Phase 1 — Normalize and classify

- [ ] Copied the address exactly, including typos
- [ ] Produced normalized variants
- [ ] Split local-part / domain
- [ ] Derived username candidates
- [ ] Labeled class and wrote why
- [ ] Role / disposable decision: continue or stop

---

## Phase 2 — Validate

- [ ] Syntax valid
- [ ] MX or A/AAAA exists (`dig MX`, `dig A`)
- [ ] Provider inferred from MX / SPF
- [ ] At least one public sighting **or** a verifier result
- [ ] Catch-all flagged if verifier says accept-all
- [ ] Did **not** probe SMTP by hand

Provider / MX notes:

```text
MX:
SPF:
DMARC:
Notes:
```

---

## Phase 3 — Search and archives

For each engine, search the quoted original **and** normalized forms.

- [ ] Google
- [ ] Bing
- [ ] DuckDuckGo
- [ ] Yandex (especially if the subject may be in CIS / adjacent spaces)
- [ ] `filetype:pdf` / `xlsx` / `csv` / `doc`
- [ ] `site:github.com` / `gitlab.com` / `linkedin.com` / `reddit.com`
- [ ] Paste-site dorks
- [ ] `intext:"@domain" -site:domain` (corporate only)
- [ ] Wayback Machine + archive.today on every important URL
- [ ] Google Alert set (if the case continues)

| URL | What it shows | Date seen | Archive link | Confidence |
| --- | --- | --- | --- | --- |
| | | | | |
| | | | | |

---

## Phase 4 — Exposure

- [ ] Have I Been Pwned (primary address)
- [ ] HIBP (every variant and every newly found address)
- [ ] Phonebook.cz / IntelX domain or address search
- [ ] Hudson Rock (or equivalent) stealer check — email and domain
- [ ] Paste-site hits reviewed in context
- [ ] **No passwords copied into notes.** Record breach *name*, *date*, *data classes* only

| Source | Date | Data classes | Pivot produced | Still live? |
| --- | --- | --- | --- | --- |
| | | | | |

Earliest public / breach date (minimum age): _______________

---

## Phase 5 — Accounts

- [ ] Epieos (or skipped for OPSEC) — name / photo / Google / Microsoft
- [ ] Holehe or user-scanner — saved raw output
- [ ] Each “found” opened and confirmed manually
- [ ] Gravatar SHA-256 avatar (`?d=404`) + profile JSON
- [ ] GHunt if Google account (investigator’s own login only)
- [ ] Hosted aggregator if authorized

| Platform | Evidence | URL | Photo / name | Confirmed live | Confidence |
| --- | --- | --- | --- | --- | --- |
| | | | | | |

---

## Phase 6 — Pivots

- [ ] Username candidates run through WhatsMyName / Maigret
- [ ] Reverse image on every avatar (Google, Yandex, TinEye)
- [ ] GitHub / GitLab commit and profile search
- [ ] osgint or equivalent if a developer handle appears
- [ ] PGP keyserver search
- [ ] Reverse WHOIS
- [ ] crt.sh on the domain
- [ ] Second emails added as **new** checklists

New identifiers (email / phone / username / URL):

```text
1.
2.
3.
```

---

## Phase 7 — Infrastructure and headers

**Domain** (skip consumer webmail)

- [ ] WHOIS created / updated / registrar / privacy
- [ ] Historical WHOIS if relevant
- [ ] SPF / DKIM / DMARC recorded
- [ ] Sister domains via reverse WHOIS
- [ ] Hunter / Phonebook / theHarvester pattern for the org

**If a message was received**

- [ ] Raw `.eml` saved
- [ ] SHA-256 of the `.eml`: ______________________________
- [ ] `From` vs `Return-Path` vs `Reply-To` vs DKIM `d=`
- [ ] `Received` chain read bottom-up
- [ ] SPF / DKIM / DMARC from `Authentication-Results`
- [ ] `Message-ID`, `X-Mailer`, `X-Originating-IP` (if any)
- [ ] Links and attachments **not** opened on the analysis box
- [ ] URLs defanged before pasting into other tools

Header verdict: [ ] authentic  [ ] spoofed  [ ] forwarded  [ ] inconclusive

---

## Phase 8 — Report

- [ ] Every claim has a source URL or archive
- [ ] Every claim has a confidence score
- [ ] Contradictions written down, not deleted
- [ ] Role / shared / family-plan caveats applied
- [ ] No password, cookie, or session material in the file
- [ ] Next investigative step is explicit (or “stop”)

### Claim log

| Claim | Sources | Confidence | Caveat |
| --- | --- | --- | --- |
| | | High / Med / Low | |

### Analyst conclusion (plain language)

```text




```

---

## Evidence pack

- [ ] Screenshots with UTC timestamps
- [ ] SingleFile / WARC of key pages
- [ ] Wayback or archive.today links
- [ ] Holehe / scanner raw output
- [ ] Header `.eml` + hash
- [ ] This checklist, filled

Retain according to your organization’s policy. Delete what you have no lawful basis to keep.
