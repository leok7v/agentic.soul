# agentic.soul

A working discipline for a coding agent, as two files. `SOUL.md` is
the discipline: how the agent reads, writes code, comments, works with
Human, waits, measures and writes prose. An agent reads it whole at
every session start.

`SOUL-INSTALL.md` installs it. An agent reads that file once and
writes its first block by hand, an 80-line script. The script writes
`SOUL.md`, the gates and a few stubs into the repo. A hash per file
checks the copy.

Both files are generated at the origin, `github.com/leok7v/agentic.devs`,
from an Open Knowledge Format 0.2 bundle. The origin is a private repo.
Do not edit the files here. Open an issue on this repo with the edit
you want. The edit goes into the origin and comes back with the next
generation. The line at the top of each file names the origin commit
it came from, for the owner's reference.

## Requirements

Git, and Python 3.9 or later. Nothing else. On Windows the name of
Python 3 differs, `python` or `py -3`, and the hook runs under the
`sh` that comes with git.

## Install only over a safe base

Install into a new repo, or into one whose tree is clean, committed
and pushed. The installer edits `CLAUDE.md` and `.gitignore` in place
and adds files, so a clean base makes every file change reversible:

    git reset --hard && git clean -fd

The one change outside the tree is the hook path in the repo's git
config, undone with:

    git config --unset core.hooksPath

The two commands above, in that order, are the whole uninstall.

A fork that mirrors an upstream can take a local install: say so to
the agent. The soul block then goes into `CLAUDE.local.md`, every
installed path goes into `.git/info/exclude`, and nothing of the
discipline reaches a branch or a pull request.

## How to install

Give the agent the raw URL of `SOUL-INSTALL.md`, or save the file
beside the repo. The raw URL is:

    https://raw.githubusercontent.com/leok7v/agentic.soul/main/SOUL-INSTALL.md

Say: read this file whole and follow it. The agent writes the block
writer by hand, checks its hash and runs it for the other files. Then
the agent fills in `okf.conf` from what it finds in the repo. The agent
runs every gate, breaks one on purpose to see it refuse, and reports.
The agent does not commit. You look first.

Then commit the install before anything else. The installed files are
untracked until you do, and a stash, a branch switch or a cancelled
`/teleport` in Claude Code removes untracked files. One session lost
an hour that way.

## What to expect after the install

- `SOUL.md` at the root, about 900 lines. The agent reads it on every
  trigger, about 12k tokens, and that cost is planned.
- `.soul/` with the block writer, the gates and the two context tools,
  Python 3 only.
- `.githooks/pre-commit`, and `core.hooksPath` set to it.
- `CLAUDE.md` with the session-start, bundle, hard-rules, comments and
  task sections between `<!-- soul -->` markers. A new repo gets a
  stub, an existing file keeps its text.
- `okf/` with `index.md`, `tasks.md`, `log.md`, an empty project
  lexicon, the type table and the read-by-tier procedure. The bundle
  grows from there, in the same commit as the code it describes.
- `okf.conf`, the values the gates read, and `tmp/` in `.gitignore`.
- `.claude/settings.json` with a `SessionStart` hook that prints the
  open tasks. A repo that ignores `.claude/` loses it. A hook in
  `~/.claude/settings.json` that runs `.soul/session_start.py` when the
  project has one does the same job for every repo:

      sh -c 'f="${CLAUDE_PROJECT_DIR:-.}/.soul/session_start.py"; [ -f "$f" ] && exec python3 "$f"; exit 0'

Then every commit runs four gates, and a commit is refused on:

- an added comment line in a covered source
- a prose fault
- a term the lexicon rejects
- a bundle that is not well formed.

Rationale and measurements go into `okf/`, not into comments.

## How to experiment

Clone a throwaway repo and install. Start a session and give one small
task. Then break a gate on purpose: add a comment line to a covered
file, stage it, and watch the hook refuse the commit. Run each gate by
hand, `python3 .soul/<gate> --root .`, and the prose gate with
`--report` for the counts without a failure. Fill in `okf.conf` before
you judge the comment gate. Its `covered` and `comment_dirs` keys
decide what it reads.

## Provenance

The bundle format is Open Knowledge Format 0.2 from Google Cloud: a
directory of markdown files with YAML frontmatter and no required
tool. The spec is
[SPEC.md in GoogleCloudPlatform/knowledge-catalog](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md).
The writing rules are ASD-STE100, Simplified Technical English, the
controlled language of aerospace maintenance manuals, at
[asd-ste100.org](https://www.asd-ste100.org/). The structural rules and
the sentence limits are taken. The dictionary is not.

## Licence

MIT. See `LICENSE`.
