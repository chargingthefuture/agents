---
description: Autonomously unblock open pull requests — fix conflicts, fix failing checks, update branches, and drive each one to merge without being told twice.
---

Work the repository's open pull requests until they merge, without asking to confirm each step.
`$ARGUMENTS` may name one or more pull-request numbers or a branch name — if it is empty, sweep
**every** open pull request that is blocked, behind, conflicted, or failing checks. The point of this
command: too many pull requests get opened and then abandoned. Opening one is the start of the job,
not the end (see *Pull requests and the merge lifecycle* in `AGENTS.md`). Do not report back with
"the pull request needs X" — do X.

## 1. Take stock

List all open pull requests with their mergeable state, check status, and review state. For each one,
decide what is in the way:

- **Conflicted** — real merge conflicts against the base branch.
- **Behind** — branch is out of date with the default branch (if the repo requires up-to-date
  branches, this silently stalls auto-merge).
- **Blocked** — failing or pending required checks, or a missing review.
- **Failing metadata checks** — a title that is not a Conventional Commit, or a body missing a field
  the repo's checks require.
- **Waiting on owner review** (risky lane) — nothing to fix; leave it, but say so.

Skip a pull request only when it is a draft someone is actively working, or it is explicitly waiting
on owner review with green checks. Everything else gets worked.

## 2. Fix conflicts

Check out the branch and bring the latest default branch into it (merge, or rebase if the branch
history is clean and small). Resolve conflicts by understanding both sides — never by blindly taking
one side. Production wins over an older mockup or copy; never "resolve" a conflict by reverting
shipped design or copy. If a conflict is genuinely ambiguous — both sides changed the same logic and
picking one loses behavior — stop on that one pull request, say exactly what the two sides do, and
keep working the others.

## 3. Fix failing checks

Read the actual failure log before touching anything. Then:

- **Real defect** (type error, lint, test, build, formatting, a drift or inventory check): fix it on
  the branch, run the same check locally until it passes, and push. A drift check that fails means the
  code and its documents disagree — fix the document to match the code, unless the code is the thing
  that is wrong.
- **Metadata check** (a semantic-title or required-field check): fix the title to a Conventional
  Commit and add whatever field the check requires to the body. If the check does not re-run on a
  description edit alone, push one empty commit to re-trigger it.
- **Environmental** (rate limit, flaky runner, a repository setting): re-run the check. Do not push
  commits to paper over an environmental failure, and say which failures were environmental.

## 4. Keep branches current until merge

Every sibling merge pushes the remaining pull requests behind. After each one merges, re-check the
others and update any that dropped behind. Expect to loop: update, wait for checks, update again. A
low-risk pull request with auto-merge enabled completes itself once it is green and current — your
job is to keep it green and current.

If a low-risk pull request has no auto-merge enabled, enable it (using the repository's default merge
method). Do not enable auto-merge on a risky pull request (money, credits, or ledger logic; auth or
access gates; data deletion; schema or migrations; new or changed API contracts; a whole new module) —
those wait for owner review, and that is the one kind of "stuck" that is correct.

## 5. Address review comments

If a pull request has unresolved review comments, verify each one against the code: fix the real ones,
and reply once with a short factual explanation for any that do not hold. Do not leave a comment
unanswered, and do not silently ignore it.

## 6. Know when a pull request is dead

If a pull request is superseded (its change already landed another way) or its branch is beyond
saving, say so and recommend closing it — with the reason — instead of pouring commits into it. Do not
close someone else's pull request without saying so in the summary.

## 7. Report back

One short summary: what merged, what is green and waiting only on owner review, what you fixed and how,
what was environmental, and anything genuinely stuck with the exact reason. Delete each branch once its
pull request merges. Plain language, no jargon, no pleasantries.
