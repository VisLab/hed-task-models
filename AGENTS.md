<!--
  COMMITTED AND PUBLIC, and the single source of instructions for every AI
  assistant working in this repo. Claude Code reads it through the @AGENTS.md
  import in CLAUDE.md; Copilot reads it directly.

  Write only what a reader needs going forward. No history: no dates, no "this
  was changed", no "used to", no rationale about superseded designs. If a fact
  is only interesting because of how the code got here, it belongs in
  .status/decisions.md, which is gitignored and is the place for that.

  Every line must be true on any machine and on Linux CI: no drive letters, no
  absolute paths, no secrets. Machine facts go in .status/local-environment.md.
  Do not restate anything that changes on its own - test counts, version pins,
  dataset counts. Point at the file that owns the fact. Target: under 200 lines.
-->

# hed-task-models

Experimental development of graph models for tasks, part of the HED (Hierarchical Event Descriptors) family of repositories. **This repo stores the models; it holds no code.** The code that produces and consumes these models lives in the `hed-vis` project.

**The repository is being populated.** The standard structure landed before the content, so this file currently states the conventions that incoming content must follow rather than describing it. Whoever moves content in updates this file in the same change: the real purpose paragraph, the Layout section, and a description of what a model file is and which `hed-vis` tools read and write it. A section below that says "none yet" is a placeholder, not a fact to preserve.

## Commands

Test framework: none. There is no code here to test; the code and its tests live in `hed-vis`. There are no build, test, or lint commands in this repository. If that ever changes, this section changes in the same commit that changes it.

## Layout

- `.status/` - working notes. **Gitignored; local to each machine.**
- Nothing else yet.

## Conventions that differ from defaults

- **Markdown headers are sentence case** - capitalize the first word only, plus proper nouns and acronyms.
- **ASCII only** in prose, comments, docstrings, and filenames: `-` not em/en dashes, `->` not arrows, `...` not an ellipsis character, straight quotes, and `|--` rather than box-drawing characters in tree diagrams. The exception is genuine data: author names, dataset titles, and recorded API responses keep whatever characters they contain.
- **Committed files carry no project history.** No dates, no "this was changed", no "previously", no phase or session labels, no pointers to plans or notes. A docstring says what the code does and the rule a reader must follow; the reader is a stranger on GitHub. Rationale about how the code got here goes in `.status/decisions.md`.
- **Nothing that ships may reference `.status/`.** It is gitignored, so such a pointer is a dead link for every reader but its author. The exception is the files whose job is to orient a tool - this file, `CLAUDE.md`, `.github/copilot-instructions.md`, `.gitignore`, and `.claude/settings.json` - which may name `.status/README.md`, `decisions.md`, `plans/`, and `local-environment.md` as places to look.
- **No committed file contains a local path or a drive letter.** Those go in `.status/local-environment.md`. This repo is public.
- **Examples use placeholders.** `TASK_NAME`, not a real identifier, in documentation and examples. Concrete identifiers belong in the model files themselves, where they are data.
- **Line endings are LF.** `.gitattributes` (`* text=auto eol=lf`) enforces this at commit time. Tools that write model files into this repo (they live in `hed-vis`) must not introduce CRLF; on Windows that means Python text-mode writers pass `newline=""`.

## Related repositories

Referred to by name, never by path.

- `hed-vis` - the code this repository relies on: the tools that build, read, and visualize the models stored here. A change to a model file's format is a change to what `hed-vis` must read - say so explicitly rather than assuming it is safe. No code is vendored here from it or from anywhere else.

## Where the thinking lives

`.status/` is gitignored, so it exists only on the machine that wrote it and never in a fresh clone or worktree.

- `.status/README.md` - the index. Read this first; it lists what is active.
- `.status/decisions.md` - why things are the way they are, and the home for anything historical. Read before proposing structural changes. Append entries; never rewrite one.
- `.status/plans/*.md` - active plans. Check the `Status:` header and the `[ ]` / `[x]` markers before starting work.
- `.status/notes/*.md` - dated records of what happened. Write-once reference material, not instructions.
- `.status/local-environment.md` - this machine's paths, interpreter, and quirks. Tool-agnostic, because more than one assistant works here. Never copy its contents into a committed file.
- IMPORTANT: do not read `.status/archive/` unless a file is named for you. Nothing new is created at the `.status/` root - new material goes in `plans/`, `prompts/`, `notes/`, or `scratch/`.

## Working agreements

- **IMPORTANT: every file written to `.status/` opens with a `For humans:` summary.** Three or four sentences, at the very top, before any other heading: what this file is, and the one or two things a person needs to take away from it. Everything below it may be written for an assistant to consume; that block is not. Write it plainly - no throat-clearing, no restating the title, no listing what the document will cover. The same applies to a long answer in a session: lead with the conclusion.
- IMPORTANT: never delete or rewrite a file under `.status/` without asking first. Appending is fine.
- IMPORTANT: temporary scripts, experiments, and one-off test files go in `.status/scratch/` - never the repository root. Anything in `scratch/` may be deleted unread.
- Show evidence, not assertions: the command you ran and its actual output. For metadata work, include counts and a sample of records.
- For a change spanning more than three files, write a plan to `.status/plans/` and stop for review before editing.
- When you are guessing about an external API's response or a data format, say so explicitly.
- Do not commit, push, or create branches unless asked.
