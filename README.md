# Reddit Content Audit: a Claude Code skill

This skill checks a Reddit post, comment, or DM against Reddit's 8 platform-wide rules (https://redditinc.com/policies/reddit-rules) before you post it. It gives you a verdict (safe / post with fixes / do not post), lists each issue with the rule it breaks, and returns a cleaned-up version.

## Install

Copy the `reddit-content-audit` folder into your Claude Code skills directory:

```bash
# for all your projects
cp -R reddit-content-audit ~/.claude/skills/

# or for one project only
cp -R reddit-content-audit /path/to/project/.claude/skills/
```

Then start a new Claude Code session.

## Use

Paste your draft and ask "audit this for Reddit", or run `/reddit-content-audit`. Name the target subreddit and paste its rules if you can, because subreddit rules decide most removals.

## Files

- `SKILL.md`: the audit steps and the report format
- `references/reddit-rules.md`: the official rule text, copied verbatim (fetched 2026-10-19), plus a checklist under each rule. The checklists are interpretation, not official Reddit text.
# reddit-content-regulation-skill
