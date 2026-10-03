# Slash commands

Every slash command from the source codebase, so a new repository starts with the same routines
instead of re-deriving them. Each file is a routine an agent follows; the owner does not have to
type the command for it to apply — the request itself is the trigger.

Copy the ones the new repo needs to `.claude/commands/`. They are plain Markdown with a
`description` in the front matter; nothing registers them beyond sitting in that folder.

## Take these into every repo

These four need nothing but a git remote and a default branch.

| Command | What it does |
|---|---|
| `br.md` | Any request that changes files. Descriptive branch off the latest default branch, surgical change, run the checks CI runs, open the pull request with title and body set at creation. Never the session's auto-generated branch. |
| `pr.md` | One pass over every open pull request that is behind, conflicted or failing. Fix each; never report that one needs something. |
| `fix.md` | The owner points at a sentence that reads badly. Rewrite it, fix every copy of it, push. Two-line reply, no explanation of what was wrong. |
| `fl.md` | The owner asks whether the chat can be archived. Answers whether work exists nowhere but the session. An open pull request is not an open task. |

**Three lines to change when you copy them**, because they name the source project's own gates:

- `br.md` and `pr.md` tell the agent to put an exact `Parity Status: web + mobile-responsive +
  android complete` line in the pull request body. That is one repository's continuous-integration
  check. Delete those lines, or swap in whatever the new repo's gate wants.
- `fix.md` names the blog repo's checks (`pnpm wiki:validate`, `pnpm wiki:spelling`, `pnpm run
  typecheck`, `pnpm wiki:build`). Swap in the new repo's own.

Everything else in the four is about how to work, not what to run, and carries over unchanged.

## Take these only with the machinery behind them

These name workflows, issue labels, inventories and generated files that exist in the source
codebase. In a repo without them they are instructions pointing at nothing, which is worse than no
command at all — an agent will follow them and invent the missing pieces. Copy one only once the
new repo actually has the thing it drives, and edit the paths.

| Command | Needs |
|---|---|
| `cr.md` | Code-review findings filed as issues with a `code-review` label. |
| `triage-bug.md` | A bug-report queue with `needs-triage` / `approved-to-build` labels. |
| `build-bug.md` | The same queue, and a build-from-issue routine. |
| `review-slice.md` | Plugins or modules to review one at a time, and somewhere to file findings. |
| `community-stats.md` | The stats workflow that files the raw numbers. |
| `product-update.md` | The product-update workflow and its publish input. |
| `user-guide.md` | Per-plugin feature inventories and a guide generator. |
| `test-script.md` | Per-plugin feature inventories and manual test scripts. |

The last seven exist because the scheduled workflows that normally do that work call a paid API,
and the account is not always funded. Each one does the same job by hand and files the same output
in the same shape, so a later funded run carries on rather than duplicating. A new repo that runs
no such workflows needs none of them.

## If you add a command later

Put it in the repo's `.claude/commands/`, list it in that repo's `CLAUDE.md` command table, and —
if it is one every repo should have — copy it back here so the next repo inherits it too.
