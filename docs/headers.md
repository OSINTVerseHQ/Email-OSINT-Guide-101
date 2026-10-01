# Email Header Analysis for OSINT and Phishing

When you have a received message, **email header analysis** beats every reverse-email lookup. Headers tell you whether the visible From address was spoofed, which infrastructure sent the mail, and which identifiers are even worth investigating.

This page is the deep dive for Phase 7 of the [Email OSINT Guide 101](../README.md).

---

## First: preserve the evidence

1. Download the message as `.eml` (or “Show original” → save the raw text).
2. Compute a SHA-256 hash of that file and write it in the case log.
3. Work on a **copy**. Do not click links or open attachments on your analysis machine.
4. Defang IOCs before they leave the evidence folder: `hxxps://`, `example[.]com`.

If you only screenshot Gmail’s pretty header view, you will lose fields you need later.

---

## How to open headers

| Client | Path |
| --- | --- |
| Gmail (web) | Open mail → ⋮ → **Show original** → copy or download |
| Outlook (desktop) | File → Properties → Internet headers |
| Outlook (web) | ⋮ → View → View message details |
| Apple Mail | View → Message → All Headers |
| Thunderbird | More → View source |
| Microsoft 365 (admin) | Message trace / header in the Security portal |

---

## The fields that matter

Read these before you look at the body.

### Identity fields (easy to forge)

| Field | Meaning | Investigative use |
| --- | --- | --- |
| `From` | Displayed sender | What the victim *saw*. Not proof of origin |
| `Sender` | On-behalf-of sender | Common in mailing lists and some phish kits |
| `Return-Path` | Envelope sender (bounce address) | The identity SPF usually evaluates |
| `Reply-To` | Where a click-reply goes | A different domain here is a classic collection trick |
| `To` / `Cc` | Recipients as claimed | BCC will not appear; look at your own envelope if you are the SOC |

**Always compare** `From`, `Return-Path`, `Reply-To`, and the DKIM `d=` domain. Four different organizations in those four slots is a story.

### Authentication fields (added by *your* side)

| Field | Meaning |
| --- | --- |
| `Authentication-Results` | SPF / DKIM / DMARC / ARC verdicts written by a receiving system you hopefully trust |
| `Received-SPF` | Older / extra SPF detail |
| `DKIM-Signature` | The actual signature; look at `d=` (signing domain) and `s=` (selector) |
| `ARC-Authentication-Results` | What a previous hop claimed, useful after forwarding |

Read the **leftmost hostname** in `Authentication-Results`. That is who performed the check. Trust your own gateway’s results more than a hop you have never heard of.

### Path fields (harder to forge at the front)

| Field | Meaning |
| --- | --- |
| `Received` | One line per hop. **Read bottom to top** |
| `Message-ID` | Originating server’s unique ID; the domain after `@` profiles the stack |
| `Date` | Claimed send time and timezone. Easy to forge; still useful vs `Received` timestamps |
| `X-Originating-IP` | Client IP, **if** a server you trust inserted it |
| `X-Mailer` / `User-Agent` | Client software |
| `X-Google-*` / `X-MS-Exchange-*` | Provider fingerprints |

Attackers can invent `From`, `Reply-To`, and even fake `Received` lines **at the bottom**. They cannot invent the `Received` line your own mail gateway wrote when it accepted the message. Start there and walk toward the sender until the hop stops being trustworthy.

---

## How to read the Received chain

1. Find the `Received` header added by **your** MX or filtering gateway.
2. Note the `from` host/IP that delivered *to you*.
3. Walk toward older hops. Stop treating hops as gospel when they leave your trust boundary.
4. Record IPs, HELO names, timestamps, and timezone offsets.
5. Geolocation of those IPs describes **mail infrastructure**, not the sender’s apartment — especially on Gmail, Microsoft 365, and Proton.

**Consumer Gmail and Microsoft 365 strip the sender’s personal IP.** If your 2014 blog post said “just grep X-Originating-IP,” retire that blog post. You will see Google or Outlook infrastructure. That still helps: it confirms the message really entered their cloud, which is useful when someone spoofs `From: ceo@yourcompany.com` from a random VPS.

---

## SPF, DKIM, and DMARC without the marketing

| Check | Question it answers | What a fail does *not* mean |
| --- | --- | --- |
| **SPF** | Was this *connecting IP* allowed to send for the **envelope** domain (`Return-Path` / `mailfrom`)? | The visible From is honest. The account was not phished. The body is safe |
| **DKIM** | Did a domain with a published key sign this body/headers, and are they unaltered? | The human is who they claim. Mailing lists often break DKIM |
| **DMARC** | Did SPF or DKIM **align** with the visible `From` domain, and what does that domain ask you to do on fail (`none` / `quarantine` / `reject`)? | A pass means “authorized infrastructure,” not “safe content.” A compromised mailbox passes all three |

**Alignment** is the concept most writeups skip. SPF can pass for `return@evil-mailer.com` while `From` says `it@yourbank.com`. That is an SPF pass and a DMARC fail. DMARC is the one that protects the *brand the user saw*.

**Forwarding** breaks SPF (the forwarder’s IP is not in the original SPF). DKIM often survives forwarding if the body was not rewritten. ARC exists to carry prior verdicts across that break. A single `spf=fail` on a forwarded newsletter is normal. A `dmarc=fail` with `p=reject` on a wire-transfer request from “the CFO” is not.

---

## Worked interpretations

### Clean, aligned, boring (good)

```text
From: billing@vendor-example.com
Return-Path: bounce@vendor-example.com
Authentication-Results: mx.yourcompany.com;
  spf=pass smtp.mailfrom=vendor-example.com;
  dkim=pass header.d=vendor-example.com;
  dmarc=pass header.from=vendor-example.com
```

Authorized infrastructure, aligned brand. Still scan the *content* — mailbox compromise looks exactly like this.

### Classic display-name / domain spoof

```text
From: "CFO Jane Doe <jane.doe@yourcompany.com>" <noreply@invoices-secure-pay.xyz>
Return-Path: bounce@invoices-secure-pay.xyz
Reply-To: payments@invoices-secure-pay.xyz
Authentication-Results: mx.yourcompany.com;
  spf=pass smtp.mailfrom=invoices-secure-pay.xyz;
  dkim=pass header.d=invoices-secure-pay.xyz;
  dmarc=fail header.from=yourcompany.com p=REJECT
```

SPF/DKIM pass for the *attacker* domain. DMARC fails for the *impersonated* domain. Investigate `invoices-secure-pay.xyz`, not Jane’s mailbox — unless you also check whether Jane’s account was used separately.

### Look-alike From domain

```text
From: it-help@yourc0mpany.com
```

Homoglyph or dropped-letter domain. Check registration date, MX, and whether DMARC even exists. Brand-new domain + look-alike + urgency in the body is the whole case.

### Proton / Gmail / Microsoft as the true origin

A DKIM `d=proton.me` (or `gmail.com`, or `outlook.com`) plus a matching `Received` hop from that provider means the mailbox on that provider sent the mail (or an app with its OAuth token did). Your remaining question is **who controls the mailbox**, which is the rest of the [main guide](../README.md) — not the header.

---

## X-headers worth a look

| Header | Hint |
| --- | --- |
| `X-Mailer`, `User-Agent` | Outlook vs Apple Mail vs a PHP script |
| `X-Originating-IP` | Only if *your* or a trusted corporate gateway inserted it |
| `X-Google-DKIM-Signature` | Google handled the message |
| `X-MS-Exchange-Organization-*` | Microsoft 365 internals |
| `X-Spam-Status`, `X-Forefront-Antispam-Report` | What your filter thought |
| `List-Unsubscribe` | Real bulk mail usually has one; many phish kits now fake it |

Absence of `List-Unsubscribe` on a “newsletter” is a weak signal. Presence is not proof of innocence.

---

## Tools

Read the raw source first. Then, if you want a drawing:

- [MXToolbox Email Header Analyzer](https://mxtoolbox.com/EmailHeaders.aspx)
- [Google Admin Toolbox Messageheader](https://toolbox.googleapps.com/apps/messageheader/)

Do not paste highly sensitive `.eml` files into random “free header tools” you have not vetted. A local Python `email` parse is enough:

```python
from email import policy
from email.parser import BytesParser
from pathlib import Path

msg = BytesParser(policy=policy.default).parsebytes(Path("sample.eml").read_bytes())
for key in ("From", "Return-Path", "Reply-To", "Message-ID", "Authentication-Results"):
    print(f"{key}: {msg.get(key)}")
print("--- Received (bottom hop is oldest) ---")
for hop in msg.get_all("Received", []):
    print(hop, "\n")
```

---

## What headers will not tell you in 2026

- The street address of a Gmail or Outlook.com user
- Whether the human typed the mail or a stolen OAuth token did (both pass auth)
- Whether a Proton user is “hiding something” (Proton is a product, not a verdict)
- The contents of attachments you have not safely detonated in a lab

---

## Related pages

- [Main email OSINT guide](../README.md)
- [Provider playbooks](providers.md)
- [Investigation checklist](checklist.md)
- [Email OSINT tools](tools.md)
