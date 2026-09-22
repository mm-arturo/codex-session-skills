# codex-session-skills

Two Codex skills for preserving project context across sessions. `wrap-up` records a verified checkpoint before you stop; `resume-work` checks that checkpoint against the current repository before continuing.

| Skill | Purpose |
| --- | --- |
| [`wrap-up`](skills/wrap-up/SKILL.md) | Save durable lessons, write `HANDOFF.md`, validate changes, and back up the session state. |
| [`resume-work`](skills/resume-work/SKILL.md) | Reload instructions and lessons, compare the handoff with Git, and identify the next safe step. |

Each skill is a directory containing a `SKILL.md`. They are written for Codex, with `AGENTS.md` instructions and PowerShell examples. No plugin or build step is required.

## Install on Windows

For your user account, run in PowerShell:

```powershell
git clone https://github.com/mm-arturo/codex-session-skills.git
New-Item -ItemType Directory -Force -Path "$HOME\.agents\skills" | Out-Null
Copy-Item -Recurse -Force -Path '.\codex-session-skills\skills\resume-work', '.\codex-session-skills\skills\wrap-up' -Destination "$HOME\.agents\skills\"
```

To use them in only one repository, copy the two directories to `<repository>\.agents\skills\` instead. Review the skill instructions and adapt the persistence files and approval rules to your project. Codex [documents these locations and the `SKILL.md` format](https://developers.openai.com/codex/skills).

Invoke them with `$wrap-up` and `$resume-work`, or describe the corresponding task. If Codex does not detect a newly installed skill, restart Codex.

## How the pair works

`wrap-up` separates durable rules from temporary session state: `AGENTS.md` holds concise standing instructions, `AGENT_LEARNINGS.md` or a project's existing ledger holds verified evidence, and `HANDOFF.md` records the current checkpoint. It checks the exact files before a commit and push. It does not silently discard changes or force-push.

`resume-work` reads the handoff and applicable instructions, then checks branch, commits, working tree, and recorded hashes. If the handoff is stale, the current repository state wins. It reports remaining work and starts only a safe, unambiguous next step.

These procedures do not replace your project's own rules. In particular, publication, external writes, backup destinations, and subagent use remain subject to the instructions active in your Codex session.

## Related project

The companion [Claude Code session skills](https://github.com/mm-arturo/claude-code-session-skills) use the same checkpoint idea with Claude Code conventions.

## Contributor and license

Contributor: [mm-arturo](https://github.com/mm-arturo). MIT licensed; see [LICENSE](LICENSE).
