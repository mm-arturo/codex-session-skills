---
name: resume-work
description: >-
  Reconstruct project state and reload durable lessons after a restart, a new
  Codex session, context loss, or a handoff. Use when the user says "resume",
  "continue", "where were we", "pick up where we left off", "load the handoff",
  or similar. Verifies the repository against the handoff and reloads applicable
  project, global, and skill knowledge before work continues.
---

# Resume work

Reconstruct state from durable evidence. Treat repository contents and verified
persistent files as authoritative; treat remembered chat context as supporting
information only.

## 1. Locate the workspace

Determine:

- current working directory;
- Git repository root, if present;
- current branch, HEAD, upstream, and remotes;
- whether a merge, rebase, cherry-pick, bisect, or conflict is active;
- `git status --short --branch`;
- the latest ten commits;
- the PowerShell version and whether the workspace is native Windows or WSL when
  that distinction affects the saved instructions.

If this is not a Git repository, use the current directory and continue with the
available handoff and learning files.

## 2. Reload persistent context

Read, when present:

1. the applicable global Codex instruction file in the effective Codex home:
   `$env:CODEX_HOME\AGENTS.md` when CODEX_HOME is set, otherwise
   `$HOME\.codex\AGENTS.md`;
2. the repository and nested `AGENTS.md` files applicable to the current working
   directory;
3. `HANDOFF.md` at the repository root;
4. `AGENT_LEARNINGS.md` or the project's canonical decisions/lessons file;
5. relevant entries from the effective global learning ledger:
   `$env:CODEX_HOME\GLOBAL_LEARNINGS.md` when CODEX_HOME is set, otherwise
   `$HOME\.codex\GLOBAL_LEARNINGS.md`;
6. each skill named as updated in `HANDOFF.md`, including its complete
   `SKILL.md` and relevant `references/LEARNINGS.md`.

Do not load an entire long learning ledger when only a few entries match the
current project, toolchain, or task.

Report the exact persistent sources loaded.

## 3. Verify rather than trust stale notes

Compare the handoff against the actual repository:

- expected branch versus current branch;
- recorded working state versus current status;
- commits made after the handoff;
- changed or missing files;
- test state;
- unresolved conflicts;
- upstream divergence;
- SHA-256 hashes recorded for global instruction and skill files.

Use Git history and hashes rather than filesystem modification time.

If the handoff and repository disagree, trust the repository and explain the
difference precisely. Never silently reset the repository to match the handoff.

If a conditional optimization's revalidation trigger has been metâ€”for example,
an operating-system, dependency, hardware, model, or tool version changedâ€”mark
the lesson as requiring validation before applying it.

Flag:

- missing persistent files;
- hash mismatches;
- global changes that were never backed up;
- stale handoff claims;
- contradictions between AGENTS files;
- a skill update that is invalid, duplicated, or outside its scope.

## 4. Reconstruct the working state

Summarize in a compact operational form:

- objective;
- last verified completed checkpoint;
- exact incomplete work;
- decisions and invariants now in force;
- durable lessons relevant to the next step;
- tests currently passing or failing;
- current risks and blockers;
- next one to three steps.

Do not repeat the whole handoff or learning ledger.

## 5. Continue safely

Start the first next step only when it is:

- unambiguous;
- local and reversible;
- consistent with current repository state;
- allowed by the active permissions;
- not dependent on an unresolved user decision.

Ask before destructive operations, publication, deployment, external writes,
new dependencies, irreversible migrations, or changes that conflict with the
handoff.

Do not delete `HANDOFF.md`. The next wrap-up replaces it after new progress is
made.

At the next verified optimization or durable correction, update the appropriate
persistent layer immediately rather than waiting for the end of the session.
