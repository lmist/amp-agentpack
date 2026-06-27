# agents/ — drop-in Claude Code reconstructions

⚠️ **Provenance / honesty note.** Unlike everything else in
`amp-reveng-latest/`, these four `.md` files were **NOT** extracted from the
latest binary (`0.0.1782494818-g890a1a`). The latest Amp no longer embeds its
subagent prompts — they're fetched server-side (see `../README.md` and
`../registry/subagents.md`).

These are the reconstructions from `lmist/re-harness` (`neo/agents/`), made from
the **last Amp version that still shipped the prompts verbatim**
(`@sourcegraph/amp` `dist/main.js`, version `0.0.1778343260-gb9a37d`). They are
included here because they're the most faithful *usable* version of Amp's agent
library — but treat them as "last known good," not "current."

| File | Reconstructed from | Suggested Claude model |
|---|---|---|
| `oracle.md` | neo `Wa0` | `claude-opus-4-8` |
| `finder.md` | neo `E55` | `claude-haiku-4-5-20251001` |
| `librarian.md` | neo `E35` | `claude-sonnet-4-6` |
| `code-review.md` | neo `hO4` + `OT6` | `claude-opus-4-8` |

> Model suggestions bumped to match what the **current** Amp actually runs:
> its Smart/Large modes are on `claude-opus-4-8` now (was Opus 4.7 when neo was
> captured). `code-tour` is omitted — it was already retired in the neo rebuild.

## Install

```bash
for f in oracle.md finder.md librarian.md code-review.md; do
  cp "$f" ~/.claude/agents/"$f"
done
```

To get the **current** verbatim prompts you'd have to capture them off the wire
(observe Amp's API traffic at runtime) rather than from the binary — out of
scope for this static carve.
