# Google Dorks for Email OSINT

Search operators are still the highest-ROI technique in **email OSINT**. Automated scrapers get blocked. You will not. Open each query in a browser, read the results, and archive what matters.

Use these **Google dorks** (they also work, with small syntax differences, on Bing and Yandex) as part of the [Email OSINT Guide 101](../README.md) workflow.

Replace `user@example.com` and `example.com` with the real values. Always try the **quoted original** and every **normalized variant** (plus-stripped, Gmail-dotless).

---

## Rules

- Run queries **manually**. Automated Google scraping violates terms of service and dies quickly.
- Start broad (`"user@example.com"`), then add operators.
- Repeat on Bing, DuckDuckGo, and Yandex. Indexes differ.
- Archive useful hits on the Wayback Machine or archive.today the same day.
- A dork that finds a password next to an email is a **lead about exposure**, not a login.

---

## Core lookups

```text
"user@example.com"
intext:"user@example.com"
intitle:"user@example.com"
inurl:"user@example.com"
```

Quoted search is mandatory. Unquoted search splits on `@` and returns junk.

---

## Documents and exports

People leave addresses in CVs, slide decks, procurement sheets, and “accidental” CSV uploads.

```text
"user@example.com" filetype:pdf
"user@example.com" filetype:doc OR filetype:docx
"user@example.com" filetype:xls OR filetype:xlsx OR filetype:csv
"user@example.com" filetype:ppt OR filetype:pptx
"user@example.com" filetype:txt OR filetype:rtf OR filetype:odt
"user@example.com" ext:log OR ext:cfg OR ext:env
```

`ext:` is more reliable on some Bing queries; `filetype:` is the Google habit.

---

## Code and developer residue

```text
"user@example.com" site:github.com
"user@example.com" site:gitlab.com
"user@example.com" site:bitbucket.org
"user@example.com" site:gist.github.com
"user@example.com" site:stackoverflow.com OR site:stackexchange.com
"user@example.com" "noreply" OR "committer" OR "author"
"user@example.com" "mailto:"
```

Also search GitHub’s own UI (code, commits, issues). Commit author emails are often not in Google’s index yet still public via the GitHub API.

---

## Social, professional, and community

```text
"user@example.com" site:linkedin.com
"user@example.com" site:x.com OR site:twitter.com
"user@example.com" site:facebook.com
"user@example.com" site:instagram.com
"user@example.com" site:reddit.com
"user@example.com" site:medium.com
"user@example.com" site:news.ycombinator.com
"user@example.com" site:keybase.io
"user@example.com" "contact" OR "reach me" OR "email me"
```

Bios and speaker pages are more common than you’d think. Conference sites are a gold mine (`site:.org` + `filetype:pdf`).

---

## Pastes, leaks, and “please ignore this dump”

```text
"user@example.com" site:pastebin.com
"user@example.com" site:justpaste.it
"user@example.com" site:ghostbin.com OR site:rentry.co
"user@example.com" "password" OR "passwd" OR "pwd"
"user@example.com" leaked OR breach OR dump OR combo
```

If you land on a page of credentials: **record the URL, the service name, and the date**. Do not copy the password into a login form. Do not upload the dump anywhere.

---

## Corporate domain harvest

When the domain is the target, not a single person:

```text
"@example.com" -site:example.com
intext:"@example.com" (contact OR email OR mailto)
intext:"@example.com" (cv OR resume OR "curriculum vitae")
intext:"@example.com" (speaker OR author OR "corresponding author")
site:example.com "email" OR "contact" OR "reach us" -inurl:contact -inurl:about
site:linkedin.com/in "@example.com" OR "example.com"
```

The `-site:example.com` operator finds press mentions, PDFs, and leaked directories the company does not control.

Guess the pattern only after you have **two real examples**:

```text
"john.doe@example.com"
"jdoe@example.com"
"j.doe@example.com"
"john_doe@example.com"
```

Hunter.io and Phonebook.cz are faster for this step; dorks catch what those indexes miss.

---

## WHOIS, resumes, and “about the author”

```text
"user@example.com" (whois OR registrant OR "abuse contact")
"user@example.com" (resume OR cv OR "curriculum vitae" OR "about the author")
"user@example.com" (pgp OR gpg OR "public key" OR fingerprint)
"First Last" "user@example.com"
"First Last" "@example.com" (email OR contact)
```

---

## Cached, related, and monitoring

```text
cache:"user@example.com"
```

Google’s `cache:` operator is unreliable in 2026 but still worth a try. Prefer:

1. Result ⋮ → Cached (when shown)
2. `https://web.archive.org/web/*/https://the-page-you-found`
3. archive.today

Set a **Google Alert** on `"user@example.com"` for any case that lasts more than a day.

---

## Engine-specific extras

| Engine | Why you still use it |
| --- | --- |
| **Google** | Best document and `filetype:` coverage |
| **Bing** | Different PDF/CSV index; sometimes shows emails Google dropped |
| **Yandex** | Stronger on Russian / CIS sites and some image-adjacent pages |
| **DuckDuckGo** | Fewer personalized filters; good second opinion |
| **GitHub search** | Commits and code Google never saw |
| **Phonebook.cz** | Not a search engine, but often faster than dorking a whole domain |

---

## Operator cheat sheet

| Operator | Meaning |
| --- | --- |
| `"..."` | Exact string. Always use for emails |
| `site:` | Limit to a host or TLD (`site:.gov`) |
| `-site:` | Exclude a host |
| `filetype:` / `ext:` | Restrict to a file type |
| `intext:` | Word must appear in the body |
| `intitle:` | Word must appear in the title |
| `inurl:` | Word must appear in the URL |
| `OR` | Either term (Google: uppercase `OR`) |
| `-word` | Exclude a word |
| `..` | Number range (`2018..2026`) — useful with years next to an address |

---

## Related pages

- [Main email OSINT guide](../README.md)
- [Investigation checklist](checklist.md)
- [Email OSINT tools](tools.md)
