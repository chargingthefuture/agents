# Working in this repo (`agents`)

`agents` is a **template, not a project.** There is no application here to build, test, or ship. It is
the single source of the baseline agent instructions that every new repository starts from — the
writing voice, the banned-term dictionary, branch naming, and the pull-request → merge → branch
cleanup lifecycle. New projects are created by copying this baseline in (see `README.md`), so every
project shares the same agent behavior from its first session.

What that means when you work here:

- **An edit here changes the baseline for every future project**, not one project's features. Do not
  treat this repo as a project. There is nothing to run, and the pull-request/merge lifecycle written
  in `AGENTS.md` is a rule for the *forked projects* — not a ceremony to perform on this repo.
- **The baseline is settled.** Do not relitigate the voice, the dictionary, the lifecycle, or branch
  naming, and do not ask the user to re-explain those concepts. Change them only when the user
  explicitly asks to change the baseline itself.
- **`AGENTS.md` is the file that gets copied into a new repo** (as its `CLAUDE.md` or `AGENTS.md`),
  together with the hook in `hooks/` and `settings.example.json`. Keep `AGENTS.md` and
  `hooks/check-no-pleasantries.mjs` in step — the hook is the source of truth for the dictionary.
- **Do not copy this file into a forked project.** It describes the template repo itself; the thing to
  copy is `AGENTS.md`.
