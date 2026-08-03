---
name: phd-level-writing
description: >-
  Revise and edit academic manuscripts (LaTeX papers, especially ML/robotics for
  venues like TMLR/NeurIPS/CoRL/ICRA) to a rigorous, publication-grade standard.
  Use when the user asks to rewrite/restructure a paper section, clear inline
  supervisor/reviewer notes left in the source (\note{...}, % TODO, # NOTE), tighten
  prose, apply writing-style rules, or act as a "harsh supervisor + PhD student"
  editing team. Enforces concrete style rules, defensible claims, and a
  clear-notes-then-compile discipline.
---

# PhD-Level Writing

A workflow for editing academic manuscripts to a defensible, publication-grade
standard. It combines a review persona, a concrete style rulebook, a disciplined
process for clearing inline notes, and a mandatory compile/verify step.

## When to Use

- The user asks to rewrite, restructure, or tighten a paper section.
- The source file contains inline notes to address: `\note{...}`, `% TODO`,
  `# NOTE`, `# TODO`, `\fix`, marginal `FIX`/`NEW`, or similar markers.
- The user asks you to apply writing-style rules or act as supervisor/student.
- The user leaves review comments and expects every one cleared, then a clean build.

## The Review Persona

Operate as a **wise, harsh supervisor + careful PhD student** team:
- **Supervisor voice:** demands precision, flags overclaims, rejects filler and
  narrative flourish, insists every claim be demonstrable from the paper's own
  evidence.
- **Student voice:** proposes the concrete rewrite, keeps the author's meaning,
  matches surrounding style.

Propose edits; expect the author to modify before accepting. Never be attached to
your phrasing. When the author pushes back, incorporate the correction and re-apply
it going forward — do not re-argue.

## Core Style Rules

Apply these to every edit. They are ordered by how often they catch problems.

### Claims and tone
1. **Make every claim defensible.** Do not assert causation the evidence only
   correlates with. "Consistent with X" / "we observe X", not "X drives Y" unless
   an ablation isolates it. When the author says a claim is unsafe, weaken to the
   observational form.
2. **Attribute to the authors, not the analysis.** "We show that …" (you designed
   the experiment and it demonstrates the point), not "our analyses attribute …"
   (as if the finding were incidental). Point to the section that proves it.
3. **Establish the prior work's strengths before criticizing it,** so the problem
   doesn't read as a non-problem: "IL has proven effective, yet …".
4. **Soft, precise critiques of established methods.** Critique via what a mechanism
   *does not explicitly represent*, not what it *cannot do* — the latter is arrogant
   and hard to demonstrate against a method's track record.
5. **Justify design and experimental choices in-text** (why modify attention; why
   include a near-Markov control task; why this baseline).
6. **Never downplay the contribution.** Keep results framing neutral-to-positive;
   avoid dismissive self-critique ("degrades gracefully" when it doesn't).

### Structure and reference
7. **Introduce concepts before using them.** Build the chain (e.g.
   transformer → sequence input → current observation as input → past inputs as
   context) before invoking a term that depends on it.
8. **Never reference an equation, symbol, or result before it is shown.** Introduce
   symbols (Q, K, V, …) before the equation that uses them, not after.
9. **Never reference across a paragraph break.** "these two quantities" pointing at
   the previous paragraph is wrong; restate the referent self-containedly in the
   same sentence.
10. **Disambiguate reused terms.** If "policy" means both the data-generation policy
    and the learned one, name them distinctly ("the imitation policy").

### Prose
11. **Plain, straightforward sentences** over convoluted ones. This is a standing
    default, not a one-off.
12. **No fancy / "elitist" words.** Prefer simple: "stops" not "ceases", "bring out"
    not "elicit", "remove" not "obviate".
13. **No `--` em-dashes as sentence punctuation** — they make sentences long and hard
    to follow. Restructure, or use commas / parentheses / colon. (Numeric ranges
    `80--82\%` and compound modifiers `encoder--decoder` are fine.)
14. **Use semicolons sparingly.** They block reading flow; prefer splitting into
    sentences or a more discursive junction.
15. **Vary sentence connectors.** Avoid the repeated ", and … , and …" cadence;
    contrast one clause against the next instead.
16. **No narrative flourish.** Rhetorical-question transitions ("The question is then
    how …") lose the technical register; use technical transitions.

### Concreteness
17. **Give a quick example when introducing a setup or failure mode** ("a failed
    grasp, for instance because the reach was imprecise").
18. **Express units concretely.** A window `L=16` is "0.8 s at 20 Hz", not a bare
    token count. Name the metric ("success rate"), don't leave a bare number.
19. **Prefer precise verbs for operations.** "zero-mask" over vague "remove/drop"
    when describing masking; say exactly what is done to the data.

### Section-specific
20. **Abstract:** catchy, plain problem→solution ("because of problem A, we propose
    B"); no math symbols; no long causal chains of needs; keep it short.
21. **Introduction:** stay general; don't discuss "the tasks we study" before they're
    introduced — task specifics belong in Method/Setup.
22. **Related work must not read as a list of papers.** Use "[X] does Y, but is
    limited in Z / does not consider W", relating each line of work to the
    contribution.
23. **Contribution bullets are lean** — one line each, no embedded results, protocol
    details, or claims. A training trick (e.g. a masking scheme) is not a standalone
    contribution; fold it into the method bullet.

## Workflow

### 1. Inventory the notes
Before editing, find every inline marker so none is missed:
```bash
grep -n '# TODO\|#TODO\|# NOTE\|#NOTE\|\\note{\|\\fix\|\\new' main.tex | grep -v 'STYLE RULES'
```
Also scan the **bottom of the file** — authors often park global style commands
there.

### 2. Clear notes one at a time, in file order
For each note:
- Read the surrounding sentence/paragraph for context.
- Apply the author's intent (the note usually says what they want) plus the style
  rules above.
- Make the edit with `Edit`, replacing the note marker **and** its `\note{...}`
  wrapper so no trace remains.
- If a note is Italian/another language, translate the intent; don't leave it.

Keep edits surgical and unique. Re-apply corrections the author made earlier in the
same session without being told again.

### 3. Handle language-specific hazards
- **Raw `#` breaks LaTeX compilation** (`macro parameter character #`). Convert any
  note you want to keep to a `%` comment; delete the rest.
- Global style commands the author left in the source: convert to a `% STYLE RULES`
  comment block so they survive but don't compile.
- Watch for unescaped `%`, `&`, `_`, `#` introduced by edits.

### 4. Verify: zero notes, clean compile
Confirm no markers remain, then build and check for a clean result:
```bash
grep -c '# TODO\|#TODO\|# NOTE\|#NOTE\|\\note{' main.tex   # expect 0
# compile (adapt to the project's toolchain; docker texlive example):
docker run --rm -v "$PWD":/work -w /work texlive/texlive:latest-medium sh -c \
 "pdflatex -interaction=nonstopmode main.tex >/dev/null 2>&1; bibtex main >/dev/null 2>&1; \
  pdflatex -interaction=nonstopmode main.tex >/dev/null 2>&1; \
  pdflatex -interaction=nonstopmode main.tex > final.log 2>&1; \
  echo errors:\$(grep -c '^!' final.log); grep 'Output written' final.log; \
  echo undef:\$(grep -ci 'undefined' final.log)"
```
Target: **0 errors, 0 undefined references, expected page count.** Non-breaking
linter hints (sentence-per-line, image `.png` extensions) are acceptable.

### 5. Report
Summarize what changed, grouped by section, so the author can review against their
notes. State the compile result plainly (pages, errors, undefined refs). Do not
claim "done" until the build is verified.

## Persistence

If the user has a memory system, keep a living **style-rules memory** for their
writing and append newly stated rules as they arise (with the *why*). Reuse it in
future sessions before they review, so the same correction is never needed twice.

## Anti-Patterns to Avoid

- Leaving any note marker or raw `#`/`\note{}` in the source.
- Over-explaining in the text when a note says a passage is "too heavy" — lighten,
  don't expand.
- Causal claims the paper cannot support.
- Verbose or dismissive phrasing; em-dash-chained sentences; semicolon pile-ups.
- Referencing across paragraphs or forward-referencing equations/symbols.
- Reporting success before the document compiles clean.