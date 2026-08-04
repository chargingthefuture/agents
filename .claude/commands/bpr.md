---
description: Do the task on a descriptive branch and open the pull request correctly the first time — descriptive name, plain title and body, and the right merge lane set at creation.
---

Do the work described in `$ARGUMENTS` on a properly named branch and open its pull request so it is
right the first time. This is the workflow this repo already mandates in `AGENTS.md` (*Branch naming*
and *Pull requests and the merge lifecycle*); this command exists because sessions keep ignoring it
and developing on the auto-generated `claude/<slug>` session branch. If `$ARGUMENTS` is empty, apply
this routine to the work already done in this session: move any commits off the session branch onto a
descriptive branch and open the pull request from there.

## 1. Branch first, before any code

- Create a descriptive branch off the latest default branch: a Conventional-Commit-style prefix plus
  a short kebab-case summary of the task (`feat/user-auth-refresh`, `fix/csv-export-dedup`,
  `docs/readme-quickstart`).
- Never develop on, commit to, or open a pull request from the auto-generated `claude/<slug>` session
  branch. If commits already exist there, move them: branch off the default branch with the
  descriptive name, cherry-pick or rebase the commits onto it, and abandon the session branch.

## 2. Do the task

- Keep the change minimal and surgical — change only what the task requires.
- Do not overwrite approved production design or copy without explicit owner approval.
- Update whatever this repo's own checks require in the same commit (inventories, generated docs,
  contracts, test scripts). A change is not done until the code and the documents it governs match.

## 3. Verify locally before pushing

Run the checks that would fail in continuous integration before you push: typecheck, lint, build,
tests, and whichever of the repo's own checks the change touches. Fix what they find. Do not push red.

## 4. Open the pull request correctly the first time

Set the title and body **at creation** so nothing goes red and needs re-triggering:

- Title: a Conventional Commit (`feat:`, `fix:`, `refactor:`, `chore:`, `docs:`, `ci:`, `perf:`,
  `test:`, `build:`, `style:`, `revert:`).
- Body: say what changed and why in plain language, and `Closes #<issue>` for any issue it resolves.
  Fill in any other fields this repo's pull-request template or checks require.
- Open it ready for review, never as a draft.
- Keep the voice plain: no jargon, no "phases", no pleasantries — the same rules as every other reply.

## 5. Pick the merge lane

- **Low-risk** — copy, styling, responsive layout, types, refactors, docs, dead-code removal,
  test-only changes: turn on auto-merge (using the repository's default merge method) right after
  opening, so it merges itself the moment checks pass.
- **Risky** — money, credits, or ledger logic; auth or access gates; data deletion; schema or
  migrations; new or changed API contracts; a whole new module: open it ready but do **not** enable
  auto-merge. Say in the body that it is waiting on owner review, and tell the owner in your summary.

## 6. Do not stop at "opened"

Watch the pull request through to merge: keep the branch up to date when it falls behind the default
branch, fix any check that goes red, and confirm it merged (low-risk) or is green and waiting on owner
review (risky). Delete the branch once it merges. Then report back in one short summary: branch name,
pull-request number, lane, and current state. Plain language, no jargon, no pleasantries.
