<p align="center">
  <strong>Make your agent speak and write in ASD-STE100 Simplified Technical English.</strong>
</p>

A Claude Code skill that shapes **every reply** into Simplified Technical English (STE): short sentences, approved words, active voice, one instruction per sentence, American spelling. It also handles the on-demand task of writing, rewriting, and checking technical documents for STE compliance.

Modeled on the always-on structure of [`ayghri/i-have-adhd`](https://github.com/ayghri/i-have-adhd), with STE rules and references adapted from [`nuelcyoung/asd-ste100`](https://github.com/nuelcyoung/asd-ste100).

## Install

<details open>
<summary><strong>Claude Code</strong></summary>

```bash
claude plugin marketplace add achinthbharadwaj/speak-ASD-STE100
claude plugin install speak-ste@speak-ste
```

Then type `/speak-ste`. The agent speaks STE for that session.

Want it on every session? `touch ~/.claude/.speak-ste-always` (see [INSTALL.md](./INSTALL.md)).

Local checkout instead of a remote:

```bash
claude plugin marketplace add /Users/achinth/Desktop/Code/speak-ASD-STE100
claude plugin install speak-ste@speak-ste
```

</details>

Install instructions for other agent-skills harnesses live in [INSTALL.md](./INSTALL.md).

## What it does

Two things at once:

1. **Speaks STE back to you.** Status updates, explanations, and error reports come back in STE. No preamble, no synonyms, no long sentences.
2. **Writes and checks STE.** Ask it to "rewrite this in STE" or "check STE compliance" and it applies the full workflow and reports each violation.

## What changes

**Before**

> Great question! It looks like your token verification might be using an older API, so you'll probably want to update the package and then you could rewrite that function.

**After**

> Cause: the `verifyToken` function uses the old `jsonwebtoken` API.
>
> 1. Install the new package: `npm install jsonwebtoken@latest`.
> 2. Open `src/auth.ts`.
> 3. Replace the `verifyToken` function (lines 42 to 58).
>
> Next: do the tests with `npm test`.

## The rules

10 rules. Full text in [SKILL.md](./skills/speak-ste/SKILL.md).

1. Use only approved words (one word, one meaning, one part of speech).
2. Keep sentences short (20 words for procedures, 25 for descriptions).
3. Use approved verb forms only (no modals, no -ing verbs).
4. Use active voice; give commands directly.
5. Put conditions first ("If X, do Y").
6. One instruction per sentence, one topic per paragraph.
7. Keep multi-word nouns to 3 words.
8. Restore omitted subjects, verbs, and articles.
9. Use American spelling.
10. Keep terminology consistent.

## How activation works

1. **Installed, not invoked.** Nothing happens. `SKILL.md` sets `disable-model-invocation: true`, so the model never applies the rules on its own.
2. **You type `/speak-ste`.** STE on for that session. "stop ste mode" or "normal english" turns it off.
3. **You touch `~/.claude/.speak-ste-always`.** A `SessionStart` hook loads the full ruleset from message one, every session.

## Files

```
skills/speak-ste/SKILL.md            The ruleset + write/check workflow
skills/speak-ste/references/         writing-rules, dictionary, checklist, background
hooks/                               SessionStart always-on hook
.claude-plugin/                      plugin.json + marketplace.json
.cursor/skills/speak-ste/SKILL.md    Synced copy for Cursor/agent-skills harnesses
```

## Tune it

Fork, edit `skills/speak-ste/SKILL.md`, then copy the change into the Cursor mirror:

```bash
cp skills/speak-ste/SKILL.md .cursor/skills/speak-ste/SKILL.md
```

## Disclaimer

This is an unofficial study and writing aid. It has no affiliation with ASD (Aerospace, Security and Defence Industries Association of Europe) or the Simplified Technical English Maintenance Group (STEMG), and it is not certified by either. ASD-STE100 is a registered EU trademark (No. 017966390). This skill paraphrases the standard's rules for teaching; it does not reproduce the specification text or the controlled dictionary in full. Download the official standard, free, from [asd-ste100.org](https://asd-ste100.org). No tool can guarantee STE compliance — the human writer signs off on the final text.

## License

MIT.
