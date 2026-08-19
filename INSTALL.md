# Install speak-ste

<details open>
<summary><strong>Claude Code</strong></summary>

### Install (from a GitHub remote)

```bash
claude plugin marketplace add achinthbharadwaj/speak-ASD-STE100
claude plugin install speak-ste@speak-ste
```

### Install (from a local checkout)

Point the marketplace at the repo root, not `.claude-plugin/`:

```bash
claude plugin marketplace add /Users/achinth/Desktop/Code/speak-ASD-STE100
claude plugin install speak-ste@speak-ste
```

Type `/speak-ste`.

### Verify

```bash
claude plugin list
```

### Update

```bash
claude plugin marketplace update speak-ste
```

### Uninstall

```bash
claude plugin uninstall speak-ste
claude plugin marketplace remove speak-ste
```

Or keep it installed and turn it off: `claude plugin disable speak-ste`.

### Always-on (optional)

A `SessionStart` hook loads the full ruleset at the start of every session, no `/speak-ste` needed:

```bash
touch ~/.claude/.speak-ste-always
```

Back to on-demand:

```bash
rm ~/.claude/.speak-ste-always
```

The hook only fires when the flag file exists, so installing the plugin changes nothing by itself. It honors `$CLAUDE_CONFIG_DIR` if you moved your config dir. "stop ste mode" still turns it off for the current session.

</details>

<details>
<summary><strong>Cursor, OpenCode, Amp, and any other agent-skills harness</strong></summary>

Works with any harness that reads agent skills. Swap `-a <agent>` for yours.

### Install (with the skills CLI)

```bash
npx skills add achinthbharadwaj/speak-ASD-STE100 -a cursor -y
npx skills add achinthbharadwaj/speak-ASD-STE100 -a opencode -y
```

### Install (by copying the folder)

```bash
git clone https://github.com/achinthbharadwaj/speak-ASD-STE100
mkdir -p ~/.cursor/skills
cp -R speak-ASD-STE100/skills/speak-ste ~/.cursor/skills/
```

New agent chat, type `/speak-ste`.

### Always-on (optional)

Paste this into your agent's persistent rules. Cursor: **Settings → Rules → User Rules**, or a project rule under `.cursor/rules/` with `alwaysApply: true`. OpenCode: `~/.config/opencode/AGENTS.md`.

```markdown
## Output style: ASD-STE100 Simplified Technical English

Speak and write every response in ASD-STE100 STE:

1. Use only approved words; one word, one meaning, one part of speech (start, not begin/commence).
2. Keep sentences short: 20 words for instructions, 25 for explanations.
3. Use approved verb forms only. No modals (can, could, may, might, should, would). No -ing verbs.
4. Use active voice. Give instructions as direct commands.
5. Put conditions first: "If X, do Y".
6. One instruction per sentence; one topic per paragraph.
7. Keep multi-word nouns to 3 words.
8. Keep the subject, verb, and articles. Do not omit them.
9. Use American spelling.
10. Keep terminology consistent.

Exceptions: write code, commands, and file content exactly as required (STE governs prose, not syntax). Confirm before destructive actions. When STE would delete the answer, keep the answer.
```

</details>

## How activation works

1. **Installed, not invoked.** In Claude Code, nothing happens: `SKILL.md` sets `disable-model-invocation: true`, so the model never sees the skill and never applies the rules on its own.
2. **You type `/speak-ste`.** Rules on for that session. "stop ste mode" or "normal english" turns them off.
3. **You touch `~/.claude/.speak-ste-always`** (Claude Code). A `SessionStart` hook loads the full ruleset from message one, every session.
4. **You add the always-on snippet above** (other harnesses). Keeps the core rules in your agent's persistent context.

## Troubleshooting

**`/speak-ste` not in autocomplete.** Restart the agent. The plugin index is read at startup.

**Always-on flag has no effect.** Update the plugin and restart. Hooks are read at startup, and the flag needs the plugin version that ships `hooks/hooks.json`.

**`claude plugin marketplace add` fails.** Use the `owner/repo` form for a remote. A local path must point at the repo root, not `.claude-plugin/`.

**Installed but replies still use long sentences.** Open a new session. If it still drifts, tighten the wording in `skills/speak-ste/SKILL.md`.
