# Email OSINT FAQ

Short answers to the questions people actually type into Google. The long version lives in the [Email OSINT Guide 101](../README.md).

---

## What is email OSINT?

Email OSINT is the use of publicly available information to investigate an email address: whether it is real, who likely controls it, which accounts it is tied to, how old and how exposed it is, and whether a message from it was spoofed. It does not mean reading someone’s inbox.

---

## How do I investigate an email address?

Classify it, prove it can exist, search the quoted address on multiple engines, check Have I Been Pwned, enumerate live registrations (Epieos + Holehe), pivot on usernames and photos, then — if you have a message — read the headers. Use the [8-phase workflow](../README.md#the-8-phase-email-osint-workflow) and the [checklist](checklist.md).

---

## How do I find someone’s social media from their email?

You cannot query Instagram or TikTok with an email the way people wish you could. You *can*:

1. See if Epieos / Holehe / user-scanner say the address is registered
2. Hash the address for a [Gravatar](https://docs.gravatar.com) photo and reverse-search it
3. Derive usernames from the local-part and run WhatsMyName / Maigret
4. Search the quoted address with `site:linkedin.com` and other [dorks](dorks.md)
5. Use a public Google-account pivot (GHunt) if it is a Google login

Two matching signals (photo + name, or profile + email on the same page) beat ten username collisions.

---

## Is email OSINT legal?

Collecting public information is generally lawful. Using it may not be, depending on purpose and jurisdiction. Email addresses are personal data under GDPR-style laws. Accessing an account, using leaked passwords, or completing a reset is illegal in almost every country. OSINT is **not** an FCRA background check and must not be used for employment, tenant, or credit decisions. This is not legal advice.

---

## What is the best email OSINT tool?

There isn’t one. The best *stack* is: search engines + Have I Been Pwned + Epieos + Holehe + Gravatar + MXToolbox, with GHunt when the address is a Google account. Paid aggregators save time; they do not replace corroboration. See the [tool directory](tools.md).

---

## Holehe vs Epieos — which should I use?

Both. **Epieos** tries to answer *who* (Google/Microsoft name and photo). **Holehe** answers *where* (live registrations). Epieos is a hosted website (they see your query). Holehe is a local CLI (sites see your IP).

---

## Does Holehe still work?

Broadly yes. Individual site modules break whenever a password-reset page changes. A “not found” is not proof. Confirm important hits by hand, at low volume, for authorized cases only.

---

## How do I know if an email address is real?

Syntax + MX (or A/AAAA) + a public sighting is enough for most investigations. Commercial verifiers help but will lie on **catch-all** domains. Do not open a raw SMTP session and probe a stranger’s server.

---

## How do I analyze email headers?

Save the `.eml`, hash it, compare `From` / `Return-Path` / `Reply-To` / DKIM `d=`, read `Received` bottom-to-top from your own gateway, then read `Authentication-Results` for SPF, DKIM, and DMARC alignment. Full walkthrough: [email header analysis](headers.md).

---

## Can I get a sender’s IP address from Gmail?

Usually no. Gmail and Microsoft 365 strip the sender’s personal IP. You will see Google or Microsoft infrastructure. That still helps you tell a real Gmail message from a spoofed `From:` line.

---

## What is a reverse email lookup?

It is a marketing name for “start with an email and find accounts, breaches, and a name.” The honest version is this guide’s workflow. Anyone promising a guaranteed legal name and home address from any Gmail is selling broker data or fiction.

---

## How do I find all emails on a company domain?

Phonebook.cz, Hunter.io domain search, theHarvester, and [corporate dorks](dorks.md) (`"@example.com" -site:example.com`). Learn the naming pattern from two sourced examples before you generate more. Mark guesses as unverified.

---

## What is the Gmail dot trick?

On **consumer** Gmail, dots in the local-part are ignored. `john.smith@gmail.com` and `johnsmith@gmail.com` are the same mailbox. Dots *do* matter on Google Workspace and on almost every other provider. Always store both forms.

---

## What does a plus sign in an email mean?

Plus-addressing. `name+shop@gmail.com` is normally an alias of `name@gmail.com`. Search both. The tag itself (`+shop`, `+press`, `+github`) tells you where they expected to use it.

---

## How do I check if an email was in a data breach?

Start with [Have I Been Pwned](https://haveibeenpwned.com). Record breach name, date, and data classes. Do not use any password you see. For infostealer exposure (malware on a device, not a website breach), use Hudson Rock’s free lookup or your licensed equivalent.

---

## What is Gravatar OSINT?

Gravatar publishes an avatar — and sometimes a profile with name, location, and linked accounts — at a URL derived from a hash of the email. Trim, lowercase, SHA-256, then request the avatar with `?d=404`. Details in the [main guide](../README.md#gravatar--the-underrated-face-pivot).

---

## Can I use this for recruiting or tenant screening?

No. That is FCRA (and similar) territory. Use a licensed consumer reporting agency. This guide is for investigations, journalism, security, and research with a lawful basis.

---

## How do I protect myself from email OSINT?

Unique aliases per site, GitHub private commit email, no public Gravatar you do not need, lock unused accounts, stop reusing your local-part as a global username, privacy WHOIS. See [Reduce your own email OSINT surface](../README.md#reduce-your-own-email-osint-surface).

---

## Related pages

- [Email OSINT Guide 101](../README.md)
- [Tools](tools.md)
- [Provider playbooks](providers.md)
- [Checklist](checklist.md)
