# Email OSINT Tools

A practical directory of **email OSINT tools** used in the [Email OSINT Guide 101](../README.md). Every row is here because it answers a specific investigative question. A 200-link graveyard is not a toolkit.

Tools die. If a row is stale, [open an issue](https://github.com/osintverse/Email-OSINT-Guide-101/issues).

**Rules that apply to every tool in this list**

- Passive / public-source use only
- Never use recovered passwords or cookies
- Never complete a password reset on a target
- Prefer local tools when the address is sensitive
- Record the exact query, timestamp, and output

---

## Quick picker

| If you need… | Start with |
| --- | --- |
| A face or a name in 30 seconds | Epieos, Gravatar |
| A live list of registered sites | Holehe, user-scanner |
| Breach timeline | Have I Been Pwned |
| Infostealer exposure | Hudson Rock free tools |
| Corporate pattern + more addresses | Hunter.io, Phonebook.cz, theHarvester |
| Google-account depth | GHunt |
| Username reuse | WhatsMyName, Maigret |
| Header authenticity | MXToolbox + your own reading |
| Graph of relationships | Maltego CE, or paper |
| One paid pane of glass | OSINT Industries or your desk’s licensed platform |

---

## Validation and reputation

| Tool | Type | Cost | What it is actually for |
| --- | --- | --- | --- |
| [Hunter.io Verifier](https://hunter.io) | Web / API | Free tier | Deliverability + catch-all + disposable flags. Treat catch-all as **unknown** |
| [Email Hippo](https://tools.emailhippo.com) | Web | Freemium | Syntax, MX, mailbox-level verdict without you touching SMTP |
| [EmailRep](https://emailrep.io) | API | Free / key | Reputation sketch: reputation, suspicious, references. Can be stale; confirm |
| [MXToolbox](https://mxtoolbox.com) | Web | Free | MX, blacklist, SPF/DKIM/DMARC, header analyzer |
| [disposable-email-domains](https://github.com/disposable/disposable) | List | Free | Daily-updated throwaway domain list |
| `dig` / `nslookup` | Local | Free | Ground truth for MX, TXT, NS, A |

---

## Search, harvest, and documents

| Tool | Type | Cost | What it is actually for |
| --- | --- | --- | --- |
| Google / Bing / Yandex / DuckDuckGo | Web | Free | Still the best email OSINT tool. See [dorks](dorks.md) |
| [Phonebook.cz](https://phonebook.cz) | Web | Free | Fast `@domain` harvest of indexed emails, URLs, subdomains |
| [Hunter.io Domain Search](https://hunter.io) | Web / API | Free tier | Corporate pattern (`first.last`) and sourced staff addresses |
| [theHarvester](https://github.com/laramies/theHarvester) | CLI | Free | Passive emails, names, subdomains from search engines, CT, APIs |
| [Intelligence X](https://intelx.io) | Web | Freemium | Pastes, documents, historical indexes beyond Google |
| [RocketReach](https://rocketreach.co) / [Snov.io](https://snov.io) | Web | Paid | Sales-intel style people → email. Use only with a lawful basis |
| [Whoxy](https://www.whoxy.com) / [ViewDNS Reverse WHOIS](https://viewdns.info/reversewhois/) | Web | Freemium | Domains registered with this email |
| [crt.sh](https://crt.sh) | Web | Free | Certificate Transparency; occasional embedded emails |
| [Wayback Machine](https://web.archive.org) | Web | Free | Pages that used to list the address |
| [archive.today](https://archive.ph) | Web | Free | Independent archive when Wayback misses |

---

## Breach and cybercrime exposure

| Tool | Type | Cost | What it is actually for |
| --- | --- | --- | --- |
| [Have I Been Pwned](https://haveibeenpwned.com) | Web / API | Free | Confirmed breaches + pastes; data classes; minimum age |
| [Hudson Rock free tools](https://www.hudsonrock.com/free-tools) | Web | Free | Whether email/domain appears in infostealer telemetry |
| DeHashed, LeakCheck, Snusbase | Web | Paid | Deeper corpora for licensed teams. Record names and classes, **not** passwords to try |
| [h8mail](https://github.com/khast3x/h8mail) | CLI | Free + paid backends | Local/orchestrated breach search for **authorized** assessments |
| [Mosint](https://github.com/alpkeskin/mosint) | CLI | Free | All-in-one email recon (breaches, socials, related addresses) if you want a single binary |

HIBP’s Pwned Passwords API uses k-anonymity so you can check *your own* password hashes without sending the secret. That is a defensive feature. It is not a reason to test a stranger’s recovered password against a login form.

---

## Account enumeration and identity

| Tool | Type | Cost | What it is actually for |
| --- | --- | --- | --- |
| [Epieos](https://epieos.com) | Web | Free limits | Google / Microsoft name + avatar; selected linked services. Third-party logs your query |
| [Holehe](https://github.com/megadose/holehe) | CLI | Free | Live “is this email registered?” across 100+ sites |
| [user-scanner](https://github.com/kaifcodec/user-scanner) | CLI | Free | Email + username scanning; bulk-friendly |
| [GHunt](https://github.com/mxrch/GHunt) | CLI | Free | Public Google account OSINT (Maps reviews, calendar, photos). Uses **your** Google login |
| [ProtOSINT](https://github.com/pixelbubble/ProtOSINT) | CLI | Free | Proton Mail existence / custom-domain MX notes. Prefer DNS MX for custom domains |
| [osgint](https://github.com/hippiiee/osgint) | CLI | Free | GitHub username ↔ email via public commits and GPG |
| [Gravatar](https://docs.gravatar.com) | API | Free | SHA-256(email) → avatar + public profile JSON |
| [OSINT Industries](https://www.osint.industries) | Web | Paid | Aggregated accounts + breaches in one report |
| [MailAccess](https://github.com/crl33/mailaccess) | CLI | Free | Self-hosted fan-out across many modules; heavier setup |

**Epieos vs Holehe** is not a contest. Epieos asks “who?” Holehe asks “where?” Run both, or run neither and do it by hand.

---

## Usernames and social correlation

| Tool | Type | Cost | What it is actually for |
| --- | --- | --- | --- |
| [WhatsMyName](https://whatsmyname.app) | Web | Free | Fast username presence check you can screenshot |
| [Maigret](https://github.com/soxoj/maigret) | CLI | Free | Deep username search, HTML report |
| [Sherlock](https://github.com/sherlock-project/sherlock) | CLI | Free | Classic username hunter; more false positives |
| [Namechk](https://namechk.com) | Web | Free | Quick visual scan, not authoritative |
| Google Lens / Yandex / [TinEye](https://tineye.com) | Web | Free | Reverse image on avatars |

Username hits are **leads**. See confidence rules in the [main guide](../README.md#confidence-scoring).

---

## Headers, DNS, and mail infrastructure

| Tool | Type | Cost | What it is actually for |
| --- | --- | --- | --- |
| [MXToolbox Header Analyzer](https://mxtoolbox.com/EmailHeaders.aspx) | Web | Free | Hop list + auth summary. Still read the raw headers yourself |
| [Google Admin Toolbox Messageheader](https://toolbox.googleapps.com/apps/messageheader/) | Web | Free | Clean Received-chain visualization |
| [DNSDumpster](https://dnsdumpster.com) | Web | Free | Visual DNS for a corporate domain |
| [SecurityTrails](https://securitytrails.com) | Web | Freemium | Historical DNS / WHOIS |
| [keys.openpgp.org](https://keys.openpgp.org) | Web | Free | PGP UIDs: names, extra emails, key age |

Header methodology: [Email header analysis](headers.md).

---

## Graphing and caseware

| Tool | Type | Cost | What it is actually for |
| --- | --- | --- | --- |
| [Maltego CE](https://www.maltego.com) | Desktop | Free / paid | Link analysis; Epieos and other transforms exist on the hub |
| [SpiderFoot](https://github.com/smicalex/spiderfoot) | Local | Free | Automated OSINT fan-out. Review every finding; it will over-collect |
| [Hunchly](https://www.hunch.ly) | Browser | Paid | Automatic evidence capture with hashes |
| SingleFile / Firefox / Chrome | Extension | Free | Save the page you are about to lose |
| Obsidian / any notes app | Local | Free | Claim log + source URLs. Use the [checklist](checklist.md) |

---

## Example local commands

These are the commands investigators actually type. They touch **public** data only.

```bash
# DNS ground truth
dig MX acme.com +short
dig TXT acme.com +short
dig TXT _dmarc.acme.com +short

# Holehe
pipx install holehe
holehe subject@acme.com

# theHarvester — passive sources
theHarvester -d acme.com -b duckduckgo,crtsh -l 200

# Gravatar (SHA-256 of lowercase trimmed email)
email="subject@acme.com"
hash=$(printf '%s' "$email" | tr '[:upper:]' '[:lower:]' | sha256sum | awk '{print $1}')
curl -sI "https://www.gravatar.com/avatar/${hash}?d=404"
```

```powershell
# Windows PowerShell — SHA-256 for Gravatar
$email = "subject@acme.com".ToLower().Trim()
$bytes = [System.Text.Encoding]::UTF8.GetBytes($email)
$sha = [System.BitConverter]::ToString([System.Security.Cryptography.SHA256]::Create().ComputeHash($bytes)).Replace("-","").ToLower()
"https://www.gravatar.com/avatar/${sha}?d=404"
```

---

## What we deliberately left out

- SMTP `VRFY` / `RCPT TO` probe scripts
- Anything that sends mail to the subject
- Credential-stuffing or “check if the password still works”
- Stealer-log marketplaces
- Browser extensions that scrape LinkedIn while you are logged in as yourself
- “People search” sites that republish broker data as fact

If a product’s main demo is “we show you the password,” close the tab.

---

## Related pages

- [Main email OSINT guide](../README.md)
- [Google dorks](dorks.md)
- [Provider playbooks](providers.md)
- [FAQ](faq.md)
