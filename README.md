# Portable agent-instructions template

A project-agnostic starter so a **new repository** keeps the same writing voice and banned-term
dictionary without re-deriving them each time. It is distilled from this codebase's agent rules,
with every project-specific rule removed (design-pass gating, product/plugin rules, schema and
migration rules, continuous-integration conventions, and the like). What stays is the part that
should be identical in every repo: the plain-voice rule, the banned-term dictionary, and a few
general process rules.

## This repo is a template, not a project

`agents` is not an application — there is nothing here to build, test, or ship. It is the single
source of the baseline agent instructions that every new repository starts from. Create a new project
by copying this baseline in (see *Use it in a new repo* below); every project then shares the same
agent behavior from its first session — the same writing voice, banned-term dictionary, branch
naming, and pull-request → merge → branch-cleanup lifecycle.

Because it is the baseline, these rules are settled and are not relitigated per project. The voice,
the dictionary, and the lifecycle are defined once, here, and inherited everywhere — there is no need
to re-explain them each time a project is created. Edit this repo to change the baseline for every
future project; edit a forked project to change only that project. The pull-request/merge lifecycle in
`AGENTS.md` is a rule the *forked projects* follow; this template repo itself is not run as a project.

## What's in this folder

| File | What it is |
|---|---|
| `AGENTS.md` | The instructions themselves. Copy into the new repo (as its `CLAUDE.md` or `AGENTS.md`). Covers the writing voice, the banned-term dictionary, branch naming, and the pull-request → merge → branch-cleanup lifecycle. Ends with a placeholder for that repo's own rules. |
| `hooks/check-no-pleasantries.mjs` | The Stop hook that enforces the voice rule and the dictionary. This file is the canonical list. |
| `settings.example.json` | The `.claude/settings.json` snippet that registers the Stop hook. |
| `.claude/commands/bpr.md` | The `/bpr` slash command: do a task on a descriptive branch and open its pull request right the first time — descriptive name, plain title and body, correct merge lane. Enforces the branch-naming and merge-lifecycle rules in `AGENTS.md`. |
| `.claude/commands/pr.md` | The `/pr` slash command: sweep the repo's open pull requests and drive each one to merge — fix conflicts, fix failing checks, update behind branches, address review comments. |
| `behavior-change-log.md` | An empty, append-only log for recording when an agent drifts from the owner's standing preferences (defined in `AGENTS.md`). Copy it into the new repo; each repo keeps its own history. |

## Use it in a new repo

1. Copy `AGENTS.md` to the new repo's root as `CLAUDE.md` (or `AGENTS.md`).
2. Copy `hooks/check-no-pleasantries.mjs` to the new repo at `.claude/hooks/check-no-pleasantries.mjs`.
3. Register the Stop hook: merge `settings.example.json` into the new repo's `.claude/settings.json`
   (create the file if it does not exist). The command path is `node .claude/hooks/check-no-pleasantries.mjs`.
4. Copy `.claude/commands/bpr.md` and `.claude/commands/pr.md` to the new repo at
   `.claude/commands/`. They give you `/bpr` (branch + open a correct pull request) and `/pr` (drive
   open pull requests to merge). Both are project-agnostic; adjust the risky-lane list or the metadata
   fields only if that repo needs different ones.
5. Copy `behavior-change-log.md` to the new repo's root. The owner's standing preferences (the locked
   time format and the no-silent-vocabulary-change rule) travel inside `AGENTS.md` already; this file
   is the empty log those preferences point at, which each repo then appends to.
6. Fill in the `<PROJECT-SPECIFIC RULES>` section at the bottom of the copied `AGENTS.md` with that
   repo's own rules. Leave the voice, dictionary, and process sections above it unchanged.

That is enough for the dictionary to apply from the first session: the model reads `CLAUDE.md` at
startup, and the Stop hook blocks any reply that breaks the voice rule and asks for a plain restatement.

## Keeping the two copies in step

`AGENTS.md` documents the dictionary for humans; `hooks/check-no-pleasantries.mjs` enforces it. If you
add or remove a term, change both. When they disagree, the hook is the source of truth.

## What was left out on purpose

Project-specific rules are not carried here — see the "What this template deliberately leaves out"
section in `AGENTS.md` for the list. Add the ones a new repo needs under that file's placeholder.
