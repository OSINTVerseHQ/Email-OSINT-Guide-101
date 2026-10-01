# Email Provider Playbooks

Different mail providers leak different public signals. Use this after you classify the domain in the [Email OSINT Guide 101](../README.md).

---

## Gmail and Google Workspace

**Why it matters.** Gmail is the most common personal address on earth. A Google account is also the login for Maps, YouTube, Calendar, Play, and “Sign in with Google” on thousands of sites.

**Consumer vs Workspace**

| Signal | Consumer Gmail | Google Workspace |
| --- | --- | --- |
| Domain | `gmail.com` / `googlemail.com` | Custom domain, MX to Google |
| Dots in local-part | Ignored (`j.smith` = `jsmith`) | Dots are real; different mailboxes |
| Plus-tags | Same mailbox (`user+tag@`) | Usually same mailbox |
| Header originating IP | Stripped | Stripped for outbound through Google |

**What to do**

1. Normalize dots and plus-tags. Search every form.
2. Gravatar + Epieos for a public photo / display name.
3. Gmail compose-window avatar (your own account, do not send).
4. **GHunt** (`pipx install ghunt`) against the address using **your** Google login. Collect only *public* data: profile photo, Maps reviews, public calendar, YouTube, Play Games.
5. Maps reviews: plot them. Look for home/work clusters, travel, and copy-paste review farms.
6. Search `"address"` with `site:youtube.com`, `site:maps.google.com`, and the [dork list](dorks.md).
7. “Sign in with Google” means Holehe-style hits on random SaaS sites are expected. They are still useful as interest/employment clues.

**Do not expect** a personal IP in headers. Do not expect GHunt to open private mail, Drive, or Photos. If a module asks you to do something that feels like accessing the target’s account, stop.

---

## Microsoft 365, Outlook.com, Hotmail, Live

**Why it matters.** Outlook.com is the other global consumer inbox. Microsoft 365 is the default corporate host.

**Signals**

- Consumer: `outlook.com`, `hotmail.com`, `live.com`, `msn.com`
- Corporate: custom domain, MX `*.mail.protection.outlook.com`
- Plus-tags work on consumer Outlook; dots are **significant**
- Epieos and similar tools sometimes recover a Microsoft display name / avatar
- Headers: originating client IP is usually stripped on consumer and M365
- `X-MS-Exchange-*` headers confirm the Microsoft stack

**What to do**

1. Treat each dotted variant as a *possible* different mailbox (unlike Gmail).
2. Run Epieos / Holehe as usual.
3. For corporate M365, harvest the domain (Hunter, Phonebook, theHarvester) and learn the pattern before guessing more staff.
4. Skype / Teams identities historically leaked from Microsoft account pivots; treat any leftover public Skype ID as a bonus, not a guaranteed 2026 API.

---

## Proton Mail

**Why it matters.** Proton is a deliberate privacy choice. That is a tradecraft note, not a guilt note. Journalists, activists, and criminals all use it.

**Domains:** `proton.me`, `protonmail.com`, `protonmail.ch`, `pm.me`, plus **custom domains**.

**Custom domain fingerprint**

```text
MX  10  mail.protonmail.ch
MX  20  mailsec.protonmail.ch
SPF include:_spf.protonmail.ch
DKIM CNAME protonmail._domainkey → *.domains.proton.ch
```

If a personal or corporate domain points here, the operator is on a **paid** Proton plan. That is the most reliable Proton OSINT fact you will get.

**What to do**

1. Classify plus-tags (`name+press@proton.me` → base `name@proton.me`).
2. Confirm custom-domain Proton use with `dig MX`, not with rumors.
3. [ProtOSINT](https://github.com/pixelbubble/ProtOSINT) exists for Proton-specific checks. Prefer DNS for infrastructure. Treat any “keyserver” existence result as **weak** — Proton has published that some public-key responses are not proof of a live mailbox.
4. Headers from real Proton mail DKIM-sign as Proton. That authenticates the *service*, not the person’s passport.
5. You will not get a sender IP. Stop looking.

**Do not** build a Selenium bot that types addresses into Proton’s compose window unless you understand you are using your own account’s UI and may violate Proton’s terms. The main guide’s workflow does not need it.

---

## Apple iCloud

**Domains:** `icloud.com`, `me.com`, `mac.com`, plus iCloud+ custom domains (`mx01.mail.icloud.com` and siblings).

**What to do**

1. Plus-tags (“Hide My Email” and manual `+`) are common. Strip and keep both.
2. Dots matter.
3. Apple IDs sometimes appear in public Find My / developer / forum posts. Search the quoted address and the local-part as a username on Apple Developer forums and Reddit.
4. Hide My Email aliases (`@icloud.com` random strings or `privaterelay.appleid.com`) are **designed** to be one-site identities. Attribution usually comes from the *site they were used on*, not from Apple.

---

## Yahoo, AOL, and older webmail

Yahoo/AOL addresses are over-represented in **old breaches** and under-represented in new social sign-up. A Yahoo address plus a 2013–2016 breach cluster often means “this is the person’s *original* internet identity.” Hunt it harder than a two-week-old Gmail.

Search pastes, old forums, Flickr, and Yahoo Groups archives. Reverse WHOIS on these addresses is disproportionately productive.

---

## Fastmail, Zoho, mailbox.org, Tuta, and other privacy / small hosts

Identify them from MX, then treat them like Proton: **little header geolocation, some custom-domain signal, same breach/account workflow.**

| MX / SPF clue | Provider |
| --- | --- |
| `inbound.fastmail.com` | Fastmail |
| `mx.zoho.com` / `mx.zoho.eu` | Zoho |
| `*.mailbox.org` | mailbox.org |
| Tuta / tutanota MX | Tuta |

A small privacy host plus a brand-new domain plus no website is a **purpose-built persona** until proven otherwise.

---

## Corporate Google Workspace or Microsoft 365

This is “email OSINT” plus org-intel.

1. Confirm MX (Google vs Microsoft vs Proofpoint / Mimecast in front).
2. Read SPF. Extra `include:`s reveal marketing tools, ticketing, and SaaS that send as the company.
3. Learn the **pattern** from two sourced addresses (Hunter, staff page, press release, Phonebook).
4. Generate candidates from public staff names (LinkedIn via `site:linkedin.com/in "Company"`, conference programs). Mark them **unverified** until a public source or a verifier that is not catch-all agrees.
5. Role addresses (`hr@`, `press@`) are org facts, not people.
6. DMARC `p=reject` means spoofing the domain is harder. It does not mean employees are who they say on LinkedIn.

Never spray guessed addresses into a contact form or a mail merge “just to see.” That is not OSINT. That is unsolicited mail.

---

## Disposable, forwarding, and alias providers

**Disposable:** Mailinator-class, guerrilla, 10-minute mail, and thousands of look-alikes. Check [disposable-email-domains](https://github.com/disposable/disposable) and look at domain age + shared MX. Attribution value is near zero unless you are studying the *campaign*.

**Forwarders:** SimpleLogin, AnonAddy, Firefox Relay, DuckDuckGo `@duck.com`, Apple Hide My Email, Fastmail aliases. The public identity is a mask. Pivot on:

- Where the alias was published
- The site-specific tag (plus-tag, subdomain)
- Any second address that appears in the same document

Do not spend your best hours trying to “unmask” the forwarder. Spend them on the GitHub commit that used the person’s real Gmail.

---

## Catch-all domains

If a verifier says **accept-all / catch-all**, you cannot prove a guessed local-part exists. Only accept addresses that appear in a document, a breach, a commit, or a human-published page.

---

## Related pages

- [Main email OSINT guide](../README.md)
- [Email header analysis](headers.md)
- [Email OSINT tools](tools.md)
- [FAQ](faq.md)
