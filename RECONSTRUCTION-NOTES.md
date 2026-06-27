# Reconstruction notes

Provenance and reconstruction details for each agent prompt in this directory.
These were previously inline HTML comments at the top of each `.md` file; they
record how each prompt was carved/reconstructed and which model it maps to.

See `README.md` for the overall provenance/honesty caveat (these are
reconstructions from the last Amp version that shipped prompts verbatim, not
extractions from the latest binary).

## oracle.md

Reconstructed from neo's `Wa0()` at `bin/main.js:5722335`.
Persona prompt verbatim from neo (template literal slots for `${workdir}`/`${root}`
have been removed since Claude Code injects this via the surrounding context).

In neo, Oracle's underlying model is settable per-deployment via the
`internal.model` config under the "oracle" key (defaulting to GPT_5_4 when no
override is set — see `ee1()` in `bin/main.js` around offset 2,067,000). Closest
Claude equivalent for "smarter, more capable model" is `claude-opus-4-8`.

## finder.md

Reconstructed from neo's `E55()` at `bin/main.js:5742548`.
neo's Finder runs as the dedicated `finder` tool with a fast model
(equivalent role to Claude Haiku here). Persona prompt verbatim aside from
removing the `${A}`/`${Q}` workdir/root template slots (Claude Code injects those
via context).

## librarian.md

Reconstructed from neo's `E35` at `bin/main.js:5658039`.

Substantive changes vs the old amp version:
- neo dropped the "create mermaid diagrams" instruction; instead it asks for
  plain-text box-drawing diagrams (since neo's renderer has no Mermaid support).
- neo invokes GitHub via first-party `read_github`/`search_github`/`commit_search`/
  `diff` tools rather than shell `gh`. That GitHub-mode suffix isn't part of E35
  itself — the tools are wired in via the Task allowlist.

This Claude Code drop-in keeps the persona prompt verbatim, swaps the diagram
instruction back to "plain-text diagrams" (matching neo's directive), and
appends a GitHub-mode appendix wired to the `gh` CLI since Claude Code lacks
native GitHub tools.

## code-review.md

Reconstructed from neo's `hO4` at `bin/main.js:5561799` (top-level system prompt)
+ neo's `OT6` skill doc (output-formatting instructions for the main agent).

In neo, code-review is a 3-stage flow:
1. Main agent invokes the `code_review` tool.
2. The reviewer (this prompt, `hO4`) runs against the diff and may fan out to
   multiple per-check Haiku calls (each driven by `o65()` which embeds a
   user-defined check from `.agents/checks/*.md` and emits `<checkResult>` XML).
3. The main agent receives the assembled findings and renders them as the
   numbered markdown list (per `OT6`).

This drop-in collapses stages 1+2 into a single Claude Code subagent and
folds the markdown-list output instruction (from `OT6`) into the prompt, so the
agent itself produces the final user-visible format. The XML `<checkResult>`
plumbing is dropped since there's no fan-out runner here.

In neo, the reviewer's underlying model is the mode's primaryModel (e.g.,
Claude Opus 4.7 in Smart mode); per-check sub-calls run on Claude Haiku 4.5.
Closest single-model Claude equivalent is `claude-opus-4-8`.
