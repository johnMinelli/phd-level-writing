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

### Extended abstracts (2-4 pages)
24. **Discursive, not inventory.** No C1/C2, F1-F11, P1/P2 label lists in prose;
    integrate findings into connected paragraphs. If internal labels exist in notes,
    translate them to plain language ("broader grasp data" not "D2").
25. **Abstract frames, Experiments proves.** Abstract gives problem, approach, and
    one or two headline trends; it does not dump numbers, knob names, or ablations.
    Full numbers live in one results table.
26. **Few citations, high relevance.** For an extended abstract prefer 5-6 directly
    load-bearing references over survey coverage. Cite the substrate you build on
    when space allows (e.g. the locomotion controller underneath the policy).
27. **Mark partiality explicitly.** An extended abstract is initial work opening
    toward expanded research, not a closed result. Say "initial steps / initial
    study / initial deployment" and name the concrete next experiment instead of
    overclaiming completeness.

### System / embodiment-transfer papers
28. **Separate general capability from our instantiation.** "A VLA could emit leg
    joints directly" is general; "our policy emits body-level commands tracked by
    the locomotion controller" is our choice. Never present our choice as the only
    possible design.
29. **Give both motives for the interface choice.** State learning-complexity
    reduction (body-level decisions, not leg-level control) AND, most importantly,
    collection-execution consistency (the same controller tracks operator and
    policy commands). One motive alone reads as incomplete.
30. **Define the task before the data splits.** Object, goal, instruction variation,
    and what each subset adds (e.g. "push reaches the same goal without grasping")
    must appear before any D1/D2/D3 or P1/P2 comparison.
31. **Present results before discussing them.** Table first with a caption that
    states what is in- vs out-of-distribution, then the trend sentence, then the
    phase/ablation figure, then the interpretation. Never interleave discussion
    without a structured presentation.
32. **Standardized section order for deployment stories.** Prefer Data Collection,
    then Training and Inference (model choice, horizon, delay handling, action
    delivery), then Experiments (data variation), then Deployment. Do not put
    tuning details after the results they enable.
33. **Offline evaluation is half the story.** Say what the offline metric is (e.g.
    FK tip error in mm, comparable to grasp tolerance), why it was chosen over a
    saturated proxy (e.g. mean joint error), and that on-robot trials remained part
    of checkpoint selection because errors compound in closed loop.
34. **Present constraints as enabling choices.** Label conventions, keep-alive
    threads, trajectory vs instantaneous delivery are positive facts that make data
    usable, not "consequential" side-effects. Explain them in common technical
    terms with rates (e.g. 5 Hz policy grid set by the slowest camera, 50 Hz
    keep-alive holding absolute pitch while zeroing stale velocity).
35. **Credit effortful construction.** "We designed a shared interface" not "we kept
    interfaces identical"; "compatible with closed-loop execution on a balancing
    base" not "that a balancing robot can execute".

### Supervisor style lessons (from inline review rounds)
Distilled from supervisor `# NOTE` / `\note{...}` comments on the legged-VLA
manuscript. These bias every future edit; when a new supervisor comment matches
one, apply the rule without being told again.
36. **Write for both communities (ML + robotics).** "Experiments" means
    architecture studies in ML and hardware tests in robotics; "deployment"
    means moving to hardware. Prefer "Offline Evaluation" vs "Hardware
    Deployment" as section names, and "closed-loop real-world experiments" in
    the abstract, so the text reaches every community.
37. **Put the money figure early.** A rollout/hardware visual on the first page
    tells the reader the topic immediately and is more impactful than a late
    reveal. Tighten subfigure gaps (`\hspace{2mm}` over `\hfill`) so the figure
    earns its space.
38. **Qualify which X on first use.** "The action space" → "the commanded action
    space of the policy"; "the task" inside Data Collection → "the robot task";
    "head-mounted" must say mounted on what ("mounted on the quadruped's
    head"). Never assume the reader carries the intended reading from a
    distant noun.
39. **Introduce mechanisms before role nouns.** "Leader" means nothing until the
    leader–follower pair is named; write "the leader device in a
    leader–follower pair". Same for any role noun that implies an unintroduced
    mechanism.
40. **Reuse established vocabulary; never coin mid-paper.** A term used once
    ("balancing base", "useful field of view") confuses more than it adds.
    Reuse what the paper already built ("the quadruped", "the forward camera
    view in which the robot can act"), and define any genuinely new term at
    first use.
41. **Gloss domain jargon for the general reader.** "Absolute joint-position
    targets", "relative delta commands integrated over time", "action chunk (a
    sequence of predicted actions)". Test each jargon term against a reader
    outside the subfield; ML-common words ("chunk") still need their object
    ("of what?").
42. **No metaphorical words in technical prose.** "Consumes" (for controllers),
    "bank" (for sets), bare "chunk", "staleness" — use literal terms
    ("expects", "set", "sequence of predicted actions", "outdated plans")
    unless the metaphor is marginal and avoids repetition.
43. **Precise verbs everywhere, not just operations.** Operators "collect"
    demonstrations (not "produce"); authors "verify" (not "check"); studies
    "examine" (not "ask"). Prefer technical verbs over discursive ones.
44. **Plain nouns: strip overspecificity, disambiguate "scale".**
    "Configuration" where "joint positions" suffices; drop modifiers that add
    no information. "Scale" is overloaded in ML — write "dataset size" /
    "demonstration count" for amount of data.
45. **Check the logic of every connector.** "While" claims contrast; if the
    relation is additive, write "and". Contrast one clause against the next
    deliberately (extends rule 15).
46. **Demote unintroduced factors to examples.** Factors never built in the
    paper (delays, contact, balance) enter as "the same physical factors, such
    as …", not as definite claims that inherit unexplained importance.
47. **Name table content in prose, with matching units.** A results paragraph
    must name what it reports ("held-out action-prediction error for grasp and
    push"), and numbers must match the table: gripper accuracy in percent in
    both header and prose ("57%", never 0.57 in one and % in the other).
48. **Spell out the takeaway; no slogans.** "At the demonstration counts
    available here, data coverage limits performance more than model capacity
    does" — not a compressed "coverage, not capacity" contraction. State both
    sides plainly so the message survives skimming.
49. **No "half of X" without naming the whole.** "Insufficient to judge
    deployment readiness because errors may compound in closed loop" — say
    what is missing and for whom, never bare "half of the evidence".
50. **Soften absolutist contribution framing.** "Practical steps", not "the
    steps necessary". The paper presents *a* route, not *the* route (mirror of
    rule 6: neither downplay nor over-absolutize).
51. **Name the referent, then cite.** "Initial body placement, the
    initial-approach phase that Fig. X shows as …" — never a bare figure
    citation as the only identifier of which phase/mode/condition is meant
    (extends rule 9).

## Workflow

### 0. General read before local fixes
Read the whole document plus all inline notes first. Supervisor notes that look
like line edits ("rephrase X", "this is unclear") often require a reasoned
structural change: a missing task definition, a tuning section placed after the
results it enables, or a discussion without a preceding table. Plan the
restructure, then edit. Never treat comments as isolated line substitutions.
A weak auto-generated redraft (e.g. `main-claude.tex`) may be mined for recalled
technicalities (latencies, horizons, keep-alive rates, depth diagnostics) but
never copied for prose; verify any reused number against code or notes.

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