# phd-level-writing

A [Claude Code](https://claude.com/claude-code) skill for revising academic
manuscripts (LaTeX / ML / robotics papers, e.g. for TMLR, NeurIPS, CoRL, ICRA)
to a rigorous, publication-grade standard.

It encodes a reusable editing workflow:

- **A review persona** — a harsh-supervisor + PhD-student editing team.
- **A concrete style rulebook** — defensible claims, plain prose, structure and
  reference discipline, section-specific rules for abstract / intro / related
  work / contributions.
- **A clear-notes discipline** — inventory every inline marker (`\note{...}`,
  `% TODO`, `# NOTE`), clear them one at a time in file order, and strip the
  markers.
- **A verify step** — compile the document and confirm zero errors, zero
  undefined references, and the expected page count before reporting success.

## Install

```
npx skills add johnMinelli/phd-level-writing
```

## Use

The skill triggers automatically when you ask Claude to rewrite or restructure a
paper section, clear inline supervisor/reviewer notes in the source, tighten
prose, or apply writing-style rules. You can also invoke it explicitly:

```
/phd-level-writing
```

## License

MIT
