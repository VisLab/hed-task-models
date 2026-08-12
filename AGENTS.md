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

Experimental development of graph models for tasks, part of the HED (Hierarchical Event Descriptors) family of repositories: directed-graph descriptions of the task structure of neuroscience and behavioral experiments, extracted from the events of BIDS datasets. **This repo stores the models; it holds no code.** The code that produces and consumes these models lives in the `hed-vis` project.

This repository is the canonical home of two things: `model_rules.md`, the numbered specification of what a model is and how one is built, and `json_examples/`, worked examples - currently one, the Sternberg working memory task. No other repository carries a current copy of either.

## Commands

Test framework: none. There is no code here to test; the code and its tests live in `hed-vis`. There are no build, test, or lint commands in this repository. If that ever changes, this section changes in the same commit that changes it.

## Layout

- `model_rules.md` - the specification: numbered rules for what a model is, how node columns are chosen, phases, trial boundaries, provenance, and reports. The authoritative statement of the format; start here.
- `json_examples/` - worked examples, one directory per task.
  - `sternberg/` - modified Sternberg working memory task, built from the BIDS EEG dataset `eeg_ds004117s_hed_sternberg` (OpenNeuro ds004117) in `hed-examples`.
    - `data/` - reference copies for the reader: the dataset README, the events sidecar, one sample run of events, and `dataset_source.json`, which records where the full source dataset lives. Models are built from the full dataset, never from this directory.
    - `models/event_type_task_role/` - the current model: the model JSON, its keymap TSV, and `reports/` (`model_summary.md`, `log.md`).
    - `models/draft/` - an early draft model that predates `model_rules.md` and does not conform to it; kept for comparison, never a format example to follow.
- `.status/` - working notes. **Gitignored; local to each machine.**

## What a model file is

A model is a JSON file describing one experiment's task structure as a directed graph, derived from the `_events.tsv` files of a BIDS dataset. Each unique combination of values in the model's chosen `columns` becomes a node; a row mapping to node `Ni` immediately followed by a row mapping to `Nj` becomes an edge carrying a count. Beyond nodes and edges, a model declares its phases and their order, its trial boundaries, the rows it excluded and why, and full provenance: which files and rows it was built from and which columns were ignored. The normative definition is `model_rules.md`; a model that does not satisfy it is wrong, not a variant.

`dataset_source.json` in an example's `data/` directory is the one machine-dependent point in the pipeline: it lists candidate dataset roots, tried in order, resolved relative to that file. The committed file carries relative roots only; a machine whose checkout layout differs records its absolute dataset root in `.status/local-environment.md` and adds it to the local copy of the roots list without committing it.

No generator code exists yet, here or anywhere. When it is written it will live in `hed-vis`, not here; this repository stores only the spec, the models, and the reference data needed to check them.

## Conventions that differ from defaults

- **Markdown headers are sentence case** - capitalize the first word only, plus proper nouns and acronyms.
- **ASCII only** in prose, comments, docstrings, and filenames: `-` not em/en dashes, `->` not arrows, `...` not an ellipsis character, straight quotes, and `|--` rather than box-drawing characters in tree diagrams. The exception is genuine data: author names, dataset titles, and recorded API responses keep whatever characters they contain.
- **Committed files carry no project history.** No dates, no "this was changed", no "previously", no phase or session labels, no pointers to plans or notes. A docstring says what the code does and the rule a reader must follow; the reader is a stranger on GitHub. Rationale about how the code got here goes in `.status/decisions.md`.
- **Nothing that ships may reference `.status/`.** It is gitignored, so such a pointer is a dead link for every reader but its author. The exception is the files whose job is to orient a tool - this file, `CLAUDE.md`, `.gitignore`, the files under `.claude/` (`settings.json`, `rules/`), and those under `.github/` (`copilot-instructions.md`, `instructions/`) - which may name `.status/` paths as places to look.
- **No committed file contains a local path or a drive letter.** Those go in `.status/local-environment.md`. This repo is public.
- **Examples use placeholders.** `TASK_NAME`, not a real identifier, in documentation and examples. Concrete identifiers belong in the model files themselves, where they are data.
- **Line endings are LF.** `.gitattributes` (`* text=auto eol=lf`) enforces this at commit time. Tools that write model files into this repo (they live in `hed-vis`) must not introduce CRLF; on Windows that means Python text-mode writers pass `newline=""`.

## Related repositories

Referred to by name, never by path.

- `hed-vis` - the code this repository relies on: the tools that build, read, and visualize the models stored here. A change to a model file's format is a change to what `hed-vis` must read - say so explicitly rather than assuming it is safe. No code is vendored here from it or from anywhere else.
- `hed-examples` - the curated BIDS example datasets the models are built from. Datasets are never copied here; each model records provenance against its source dataset, and `dataset_source.json` says where that dataset lives.

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
