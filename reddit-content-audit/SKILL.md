---
name: reddit-content-audit
description: Audit and validate a Reddit post, comment, title, DM, or reply draft against Reddit's platform-wide rules (Rules 1–8) and spam/self-promotion norms before posting. Use whenever the user shares content intended for Reddit and asks to check, audit, validate, review, or "is this safe to post", or mentions Reddit rules, subreddit rules, ban risk, self-promotion, shadowban, or Rule 2.
---

# Reddit Content Audit

Validate content the user plans to post on Reddit against the official Reddit Rules, flag every violation and risk, and return a fixed version.

The full rule text (verbatim from redditinc.com/policies/reddit-rules) and detailed check lists are in `references/reddit-rules.md`. **Read that file before every audit.** Don't audit from memory.

## Inputs to collect

Audit whatever the user gives. If any of these are missing and would change the verdict, note them as UNVERIFIED in the report; don't block on them:

1. **The content**: title, body, any links, image/video descriptions, flair
2. **Target subreddit(s)**: needed to check community rules (Rule 2). If the user pastes the sub's rules, apply them strictly.
3. **Content type**: post, comment, reply, DM, cross-post
4. **Author's relationship to anything promoted**: owner, employee, affiliate, none
5. **Account context** if relevant: age, karma, previous posting about the same product, prior bans

## Audit procedure

1. Read `references/reddit-rules.md`.
2. Go through **all eight rules in order** and run every check under each one against the content. Also check the "soft-fail signals" list.
3. For each issue, record:
   - Rule number
   - Severity:
     - **CRITICAL**: clear violation, likely removal or suspension. Rules 3, 4, and 7 hits are almost always critical, and a Rule 4 hit always is.
     - **HIGH**: probable violation or strong spam signal, likely mod or automod removal, and it builds toward a ban
     - **MEDIUM**: gray area; depends on the sub or account history
     - **LOW**: style or soft signal that hurts reception but isn't a violation
   - The exact offending text (quote it)
   - Why it violates or risks the rule
   - A concrete fix (rewritten text, not vague advice)
4. Apply the "spirit, not just letter" test. Ask: would a mod who saw the author's full post history consider this authentic participation or disguised marketing?
5. Produce a cleaned version that keeps the author's intent and voice and fixes every CRITICAL and HIGH issue. If the core purpose of the content is itself the violation (e.g. it exists only to drive traffic to a product in a no-promo sub), say so plainly. Don't polish it into a sneakier version, because disguised promotion is a Rule 2 + Rule 5 violation in itself.
6. For Rule 4 or other zero-tolerance hits, don't provide a rewrite that keeps the element. State that it can't be posted.

## Output format

```
## Reddit Audit: <short label>

**Verdict:** ✅ SAFE TO POST | ⚠️ POST WITH FIXES | ❌ DO NOT POST
**Risk score:** <0–10>  (0 = clean, 10 = near-certain removal/suspension)
**Target:** r/<sub> (rules checked: yes / no: UNVERIFIED)

### Rule-by-rule
| # | Rule | Status | Notes |
|---|------|--------|-------|
| 1 | Remember the human | ✅ / ⚠️ / ❌ | ... |
| 2 | Community rules / spam / manipulation | ... | ... |
| 3 | Privacy | ... | ... |
| 4 | Minors | ... | ... |
| 5 | Authenticity | ... | ... |
| 6 | Labeling (NSFW/spoiler/flair) | ... | ... |
| 7 | Legal | ... | ... |
| 8 | Site integrity | ... | ... |

### Issues
1. **[SEVERITY] Rule N**: "<quoted text>"
   Why: ...
   Fix: ...

### Unverified / need from you
- ...

### Cleaned version
<title>
<body>

### Posting notes
- Required flair / NSFW / spoiler tags
- Disclosure line to include (if affiliated)
- Timing and cross-post spacing, and whether to stay in the comments without pitching
```

Verdict logic:
- Any CRITICAL → ❌ DO NOT POST (as written)
- Any HIGH, or unverified sub rules on promotional content → ⚠️ POST WITH FIXES
- Only LOW, or nothing → ✅ SAFE TO POST

## Principles

- Quote the official rule text when you cite a rule; mark your own enforcement reasoning as interpretation.
- Be strict. A false "safe" can cost the account, while a false flag costs one edit.
- Disclosure fixes most authenticity problems ("I built this", "I work at X"). It doesn't fix a sub that bans promotion.
- Never suggest ban evasion, alt accounts, vote coordination, or ways to slip past automod or filters as a "fix".
- Subreddit rules can be stricter than platform rules, never looser. If the user hasn't provided them, say what kinds of sub rules would change the verdict.
- The official rules page is short, and Reddit updates it. If the user asks to refresh, re-fetch https://redditinc.com/policies/reddit-rules (WebFetch is blocked for this domain, so use `curl` with a browser user agent) and update `references/reddit-rules.md`.
