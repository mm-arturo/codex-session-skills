---
name: wrap-up
description: >-
  Persist verified session knowledge and current work before closing Codex,
  restarting the computer, switching projects, or ending substantial work. Use
  when the user says "wrap up", "save session", "prepare for restart",
  "checkpoint", "closing the shell", or similar. Includes a retrospective,
  durable-learning updates, handoff creation, validation, commit, and backup.
  Do not use for an ordinary commit request that does not require session
  persistence.
---

# Session wrap-up

Make volatile session state durable without polluting long-lived instructions
with guesses, one-off details, or duplicated rules.

## Persistence standard

A lesson is not considered persisted until all of the following are true:

1. It has been written to the correct durable file.
2. The written file has been re-read and checked for accuracy.
3. Repo-specific changes are committed and pushed when a safe remote is
   available, or explicitly reported as local-only.
4. The evidence, scope, and any revalidation condition are recorded.
5. Any failed write, permission problem, or unpushed change is reported
   explicitly. Never claim success after a partial write.

## Non-negotiable safeguards

- Never persist secrets, credentials, tokens, private keys, seed phrases,
  passwords, cookies, private URLs, raw personal data, or secret-bearing command
  output.
- Do not persist transient process IDs, temporary ports, short-lived URLs,
  timestamps with no future value, raw logs, or the full transcript.
- Never force-push, reset, clean, rewrite history, delete files, discard changes,
  or overwrite another contributor's work unless the user explicitly requested
  that action.
- Never use `git add .` or `git add -A`. Stage explicit, reviewed paths.
- Do not create a new skill without user approval.
- Do not alter a skill's `name` or `description` unless its actual trigger scope
  changed.
- If a global path is not writable, request the required permission. If
  permission is denied, write `PERSISTENCE_PENDING.md` at the repository root
  with the exact proposed patch and clearly report that the change is not yet
  permanent.
- Prefer a small correct instruction over a broad rule that may become false.

## 1. Establish the actual workspace state

Determine and report:

- current working directory;
- repository root, if this is a Git repository;
- branch, HEAD, upstream, and remotes;
- `git status --short --branch`;
- unstaged and staged file lists;
- important untracked files;
- whether a merge, rebase, cherry-pick, bisect, or conflict is active;
- active goal or current task, if one exists;
- relevant background processes or services that must be restarted later;
- the PowerShell version and whether the work is running in native Windows or
  WSL when that distinction affects commands, paths, permissions, or results.

If this is not a Git repository, skip Git-only operations but still perform the
retrospective, durable-learning update, and handoff.

Before editing anything under `$HOME\.agents` or the effective Codex home
(`$env:CODEX_HOME` when set, otherwise `$HOME\.codex`), verify that it is
writable in the current Codex permission profile. Request approval rather than
silently skipping global persistence.

## 2. Run a session retrospective

Review the current conversation, user corrections, changed files, command
results, test output, benchmarks, abandoned approaches, and existing session
notes.

For a substantial session, delegate one read-only retrospective review to a
subagent. Ask it to identify:

- durable lessons the main agent may have missed;
- proposed lessons that are unsupported or too specific;
- optimizations whose scope, evidence, or revalidation trigger is incomplete;
- repeated mistakes that should become a standing instruction;
- procedures in an existing skill that demonstrably need correction.

The main agent remains responsible for every persistent edit.

Candidate durable knowledge includes:

- a correction the user made that should affect future work;
- a non-obvious environment, dependency, platform, drive, path, execution-policy,
  line-ending, shell, version, or tool quirk;
- a wrong assumption disproved during the session;
- a workflow improvement that was tested successfully;
- a performance optimization with a measured or otherwise verifiable benefit;
- a failed approach that future sessions are likely to retry;
- an invariant, interface contract, or project decision that future work must
  preserve;
- a reliable diagnostic or validation command;
- a skill procedure that was incomplete, unsafe, ambiguous, or inefficient.

Persist an item only when it is:

1. verified during this session;
2. non-obvious;
3. likely to recur;
4. actionable;
5. scoped to the environment or project where it is valid.

Do not promote speculation, a single unexplained observation, generic advice,
or an optimization whose supposed improvement was not checked.

For every optimization, preserve:

- the previous approach or baseline;
- the changed approach;
- the observed outcome and metric, when available;
- operating system, hardware, dependency, model, or tool version when relevant;
- the validation command or evidence;
- the scope in which it applies;
- the rollback or safer fallback;
- the condition that should trigger revalidation.

For a failed approach, record what was tried, the observable failure, and the
conditions under which it should not be retried.

## 3. Route each lesson to the correct durable layer

Use the narrowest authoritative destination that will reliably be loaded later.

### Project standing rule

Write a concise imperative rule to the nearest applicable `AGENTS.md` when the
instruction must affect future work automatically.

Examples:

- required build, test, lint, or validation command;
- codebase invariant;
- repository convention;
- recurring user correction;
- routing rule that prevents wasted exploration.

Keep `AGENTS.md` short. Check for contradictions and near-duplicates first.
Rewrite an existing rule instead of appending another version.

### Project detailed learning

Write evidence and context to `AGENT_LEARNINGS.md` at the repository root, unless
the project already has a canonical decisions, experiments, or lessons file.
Prefer the existing canonical file when one exists.

Use this entry format:

    ## YYYY-MM-DD â€” Short title
    - Status: verified | conditional
    - Scope:
    - Lesson:
    - Evidence:
    - Applies when:
    - Revalidate when:
    - Fallback or rollback:
    - Related files and commands:

`AGENT_LEARNINGS.md` stores evidence and rationale. `AGENTS.md` stores only the
short active rule.

### Global standing rule

Write a concise cross-project instruction to:

    $env:CODEX_HOME\AGENTS.md when CODEX_HOME is set; otherwise
    $HOME\.codex\AGENTS.md

Use this only when the rule genuinely applies across repositories. Before
changing a global file that is not version-controlled, create a timestamped
backup and retain the backup path in the final report.

### Global detailed learning

Write cross-project evidence and context to:

    $env:CODEX_HOME\GLOBAL_LEARNINGS.md when CODEX_HOME is set; otherwise
    $HOME\.codex\GLOBAL_LEARNINGS.md

Do not turn machine-specific facts into universal rules. State their machine,
operating-system, tool, or version scope. Before changing a global file that is
not version-controlled, create a timestamped backup.

### Existing skill improvement

Update the relevant `SKILL.md` only when the session verified that its reusable
procedure should change.

Before editing a skill:

1. Read the complete existing skill.
2. Confirm the lesson belongs to that skill's procedure.
3. Check for an equivalent existing instruction.
4. If the skill is outside version control, create a timestamped backup.
5. Apply the smallest sufficient patch.
6. Keep the YAML frontmatter valid and the file UTF-8.
7. Put detailed evidence in `references/LEARNINGS.md` when it would make
   `SKILL.md` unnecessarily long.
8. Re-read the complete edited skill and inspect its diff.
9. Record the old behavior, new behavior, reason, evidence, and date.

A skill may improve itself only after an observed failure, user correction, or
verified procedural improvement. Do not self-modify merely to rephrase prose.

### New workflow

When a genuinely new repeatable workflow is identified, describe the proposed
skill, triggers, inputs, outputs, and evidence, but ask the user before creating
it.

### Current session state

Put only temporary continuity information in `HANDOFF.md`. Do not use a handoff
as the sole storage location for a durable rule.

## 4. Verify persistent edits before committing

For every file changed during the persistence pass:

- re-read the relevant section;
- confirm the rule is supported by this session;
- confirm the scope is explicit;
- check for duplicates and contradictions;
- make sure no secret or private value was copied;
- inspect the exact diff.

List every persistent change with:

- path;
- persistence layer;
- concise description;
- evidence;
- whether it is repo-backed, globally local, or pending permission.

If nothing passes the quality bar, say so and do not manufacture a lesson.

## 5. Write `HANDOFF.md` before the commit

Create or replace `HANDOFF.md` at the repository root. Do this before staging,
committing, and pushing so the handoff is included in the backup.

Use an ISO-8601 timestamp with timezone and include:

- repository path and working directory;
- branch and upstream;
- active goal and its status;
- work completed this session;
- work still in progress and the exact file, function, section, command, or
  artifact where it stopped;
- important decisions and invariants;
- next one to three concrete steps;
- tests, checks, and benchmarks run, with exact commands and results;
- known failures, risks, unresolved questions, and intentionally skipped checks;
- required environment setup, virtual environment, environment variables by
  name only, services, ports, PowerShell profile assumptions, and commands to
  resume;
- intentionally uncommitted files and why;
- standing rules, learning ledgers, and skills changed during the session;
- SHA-256 hashes for each changed global instruction or global skill file;
- whether global files are version-controlled or only stored locally.

Replace stale state rather than appending indefinitely. Git history preserves
older handoffs.

## 6. Validate the work

Use the project's existing instructions to choose checks. Run, as applicable:

- targeted tests;
- linting;
- type checking;
- formatting checks;
- a smoke test;
- `git diff --check`.

If the full test suite is too slow, run the highest-value targeted checks and
record exactly what was skipped and why. A failing test does not justify hiding
or discarding the state; record it and use a work-in-progress commit when needed.

## 7. Guard secrets and stage explicitly

Inspect staged and unstaged diffs for secrets and accidental generated files.
Use the repository's secret scanner when one exists.

Never stage:

- `.env` or local credential files;
- private keys or wallets;
- secret-bearing logs;
- ignored files unless the user explicitly approved that exact file;
- unrelated personal files.

If a secret is already tracked or appears in commit history, stop and warn. Do
not commit or push until the exposure is handled.

Stage only explicit reviewed paths, including `HANDOFF.md`, durable project
learning files, and applicable `AGENTS.md` changes.

## 8. Commit and back up

Create a descriptive commit for the meaningful session state.

- Use `wip:` when the work is incomplete or checks fail.
- Do not rewrite existing commits merely to make the history prettier.
- If global configuration files are maintained in a separate Git repository,
  commit and push them separately with an appropriate message.
- If global files are not version-controlled, preserve timestamped backups and
  report that they are durable on this machine but not remotely backed up.

Push the current branch when a safe upstream is already configured.

If no upstream exists, create one only when `origin` exists and the branch is an
appropriate non-protected working branch. Otherwise ask before publishing it.
Never force-push.

## 9. Final verification

After commit and push:

- run `git status --short --branch`;
- confirm the intended commit is HEAD;
- confirm whether the branch is ahead of or behind its upstream;
- verify that `HANDOFF.md` and project learning changes are present in the
  committed tree;
- re-read every changed global `AGENTS.md` or `SKILL.md`;
- verify the recorded SHA-256 hashes for global files;
- report any file that remains uncommitted, unpushed, local-only, or pending.

Finish with:

- repository and branch;
- commit hash and message;
- push result;
- test/check summary;
- exact persistent files changed and why;
- global changes and whether they have remote backup;
- remaining risks or author decisions;
- exact resume command:

      Set-Location -LiteralPath "<repository-root>"
      codex resume --last

Also note that `codex resume --all` can find sessions from another working
directory.
